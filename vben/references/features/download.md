# 文件下载最佳实践

## 核心原则

文件下载必须使用 `requestClient.download()` 方法，绕过响应拦截器处理。

## 标准实现

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

## 关键点

| 配置 | 说明 |
|------|------|
| `responseReturn: 'raw'` | 获取完整 Axios 响应（含 headers） |
| `method: 'POST'` | 支持 POST 请求 |
| `as any` | 类型断言避免编译错误 |

## API 参考

- `requestClient.download()`: [downloader.ts](packages/effects/request/src/request-client/modules/downloader.ts)
- `downloadFileFromBlob()`: [download.ts](packages/@core/base/shared/src/utils/download.ts)
