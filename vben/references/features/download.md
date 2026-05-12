# 文件下载最佳实践

## 核心原则

文件下载使用 `@vben/utils` 提供的工具函数，不要手写 `<a>` 标签或 `window.open`。

## 工具函数一览

| 函数 | 场景 |
|------|------|
| `downloadFileFromUrl` | 通过 URL 下载（支持跨域） |
| `downloadFileFromBlob` | 通过 Blob 下载 |
| `downloadFileFromBlobPart` | 通过 BlobPart 下载 |
| `downloadFileFromBase64` | 通过 Base64 下载 |

## 场景一：Blob 响应下载（后端直接返回文件流）

```typescript
import { downloadFileFromBlob } from '@vben/utils';
import { requestClient } from '#/api/request';

export async function exportProductModelApi(id: string) {
  const response = await requestClient.download(
    `/v1/products/${id}/exportModel`,
    {
      method: 'POST',
      responseReturn: 'raw',
    },
  ) as any;

  const blob = response.data as Blob;
  const contentDisposition = response.headers?.['content-disposition'] || '';
  let fileName = 'export.json';

  if (contentDisposition) {
    const match = contentDisposition.match(
      /filename\*?=['"]?(?:UTF-8'')?([^;\n"']+)/i,
    );
    if (match?.[1]) {
      fileName = decodeURIComponent(match[1]);
    }
  }

  downloadFileFromBlob({ fileName, source: blob });
  return blob;
}
```

## 场景二：预签名 URL 下载（先获取下载链接再下载）

适用于对象存储（MinIO/S3）预签名 URL 场景：先调接口获取带签名的临时 URL，再触发下载。

```typescript
import { downloadFileFromUrl } from '@vben/utils';

async function handleDownload(fileUrl: string, fileName: string) {
  const res = await presignDownloadUrlApi(fileUrl);
  if (res?.url) {
    downloadFileFromUrl({
      fileName,
      source: res.url,
    });
  }
}
```

## 关键点

| 配置 | 说明 |
|------|------|
| `responseReturn: 'raw'` | 获取完整 Axios 响应（含 headers） |
| `method: 'POST'` | 支持 POST 请求 |
| `as any` | 类型断言避免编译错误 |
| `downloadFileFromUrl` | 内部处理跨域、Chrome/Safari 兼容、`<a>` 标签创建和清理 |
| 不要用 `window.open` | 预签名 URL 用 `window.open` 不会触发下载，只会在新标签打开 |

## API 参考

- `requestClient.download()`: [downloader.ts](packages/effects/request/src/request-client/modules/downloader.ts)
- `downloadFileFromUrl()`: [download.ts](packages/@core/base/shared/src/utils/download.ts)
- `downloadFileFromBlob()`: [download.ts](packages/@core/base/shared/src/utils/download.ts)
