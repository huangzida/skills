# String Functions Reference

A comprehensive guide to es-toolkit's string manipulation functions.

## camelCase

Converts a string to camelCase.

**Function Signature:**
```typescript
function camelCase(str: string): string
```

**Description:** Transforms a string into camelCase format, where words are concatenated with the first letter of each word (except the first) capitalized.

**Example:**
```javascript
import { camelCase } from 'es-toolkit';

camelCase('foo bar');        // 'fooBar'
camelCase('foo-bar');        // 'fooBar'
camelCase('__FOO_BAR__');    // 'fooBar'
```

---

## capitalize

Capitalizes the first character of a string.

**Function Signature:**
```typescript
function capitalize(str: string): string
```

**Description:** Converts the first character of a string to uppercase and the rest to lowercase.

**Example:**
```javascript
import { capitalize } from 'es-toolkit';

capitalize('fred');     // 'Fred'
capitalize('Fred');     // 'Fred'
capitalize('FREDD');    // 'Fredd'
```

---

## constantCase

Converts a string to CONSTANT_CASE.

**Function Signature:**
```typescript
function constantCase(str: string): string
```

**Description:** Transforms a string into CONSTANT_CASE format, where words are separated by underscores and all letters are uppercase.

**Example:**
```javascript
import { constantCase } from 'es-toolkit';

constantCase('foo bar');        // 'FOO_BAR'
constantCase('fooBar');         // 'FOO_BAR'
constantCase('--foo-bar--');    // 'FOO_BAR'
```

---

## deburr

Converts a string by replacing special characters with their ASCII equivalents.

**Function Signature:**
```typescript
function deburr(str: string): string
```

**Description:** Removes diacritical marks from Latin characters, converting characters like `é` to `e`, `ñ` to `n`, etc.

**Example:**
```javascript
import { deburr } from 'es-toolkit';

deburr('déjà vu');        // 'deja vu'
deburr('café');           // 'cafe'
deburr('naïve');          // 'naive'
```

---

## escape

Escapes special characters in a string for HTML.

**Function Signature:**
```typescript
function escape(str: string): string
```

**Description:** Converts characters `&`, `<`, `>`, `"`, and `'` to their corresponding HTML entities.

**Example:**
```javascript
import { escape } from 'es-toolkit';

escape('fred, barney, & pebbles');    // 'fred, barney, &amp; pebbles'
escape('<div>');                       // '&lt;div&gt;'
```

---

## escapeRegExp

Escapes special characters in a string for use in regular expressions.

**Function Signature:**
```typescript
function escapeRegExp(str: string): string
```

**Description:** Escapes the characters `^$,.\*+?{}[]()|#` with backslashes for safe use in regex patterns.

**Example:**
```javascript
import { escapeRegExp } from 'es-toolkit';

escapeRegExp('[lodash](https://lodash.com)');    // '\[lodash\]\(https://lodash\.com\)'
escapeRegExp('test.value+special*chars');         // 'test\\.value\\+special\\*chars'
```

---

## kebabCase

Converts a string to kebab-case.

**Function Signature:**
```typescript
function kebabCase(str: string): string
```

**Description:** Transforms a string into kebab-case format, where words are separated by hyphens and all letters are lowercase.

**Example:**
```javascript
import { kebabCase } from 'es-toolkit';

kebabCase('Foo Bar');        // 'foo-bar'
kebabCase('fooBar');         // 'foo-bar'
kebabCase('__FOO_BAR__');    // 'foo-bar'
```

---

## lowerCase

Converts a string to lowercase with spaces between words.

**Function Signature:**
```typescript
function lowerCase(str: string): string
```

**Description:** Converts a string to lowercase and separates words with spaces.

**Example:**
```javascript
import { lowerCase } from 'es-toolkit';

lowerCase('--Foo-Bar--');    // 'foo bar'
lowerCase('fooBar');         // 'foo bar'
lowerCase('__FOO_BAR__');    // 'foo bar'
```

---

## lowerFirst

Converts the first character of a string to lowercase.

**Function Signature:**
```typescript
function lowerFirst(str: string): string
```

**Description:** Changes only the first character of the string to lowercase, leaving the rest unchanged.

**Example:**
```javascript
import { lowerFirst } from 'es-toolkit';

lowerFirst('Fred');     // 'fred'
lowerFirst('FRED');     // 'fRED'
lowerFirst('fred');     // 'fred'
```

---

## padEnd

Pads a string from the end to a given length.

**Function Signature:**
```typescript
function padEnd(str: string, length: number, chars?: string): string
```

**Description:** Pads the end of a string with specified characters (default: space) until it reaches the given length.

**Example:**
```javascript
import { padEnd } from 'es-toolkit';

padEnd('abc', 6);           // 'abc   '
padEnd('abc', 6, 'x');      // 'abcxxx'
padEnd('abc', 6, 'xyz');    // 'abcxyz'
padEnd('abc', 3);           // 'abc'
```

---

## padStart

Pads a string from the start to a given length.

**Function Signature:**
```typescript
function padStart(str: string, length: number, chars?: string): string
```

**Description:** Pads the beginning of a string with specified characters (default: space) until it reaches the given length.

**Example:**
```javascript
import { padStart } from 'es-toolkit';

padStart('abc', 6);           // '   abc'
padStart('abc', 6, 'x');      // 'xxxabc'
padStart('abc', 6, 'xyz');    // 'xyzabc'
padStart('abc', 3);           // 'abc'
```

---

## parseJSON

Parses a JSON string safely.

**Function Signature:**
```typescript
function parseJSON(str: string, reviver?: (this: any, key: string, value: any) => any): any
```

**Description:** Safely parses a JSON string, returning the parsed value or a default value on error.

**Example:**
```javascript
import { parseJSON } from 'es-toolkit';

parseJSON('{"a": 1}');                    // { a: 1 }
parseJSON('[1, 2, 3]');                   // [1, 2, 3]
parseJSON('invalid', { default: null });  // null
```

---

## snakeCase

Converts a string to snake_case.

**Function Signature:**
```typescript
function snakeCase(str: string): string
```

**Description:** Transforms a string into snake_case format, where words are separated by underscores and all letters are lowercase.

**Example:**
```javascript
import { snakeCase } from 'es-toolkit';

snakeCase('Foo Bar');        // 'foo_bar'
snakeCase('fooBar');         // 'foo_bar'
snakeCase('--foo-bar--');    // 'foo_bar'
```

---

## template

Compiles a string template into a function.

**Function Signature:**
```typescript
function template(string: string, options?: TemplateOptions): ((data?: object) => string)
```

**Description:** Compiles a template string with ES6 template literal syntax into a function that can be called with a data object.

**Example:**
```javascript
import { template } from 'es-toolkit';

const compiled = template('Hello <%= name %>!');
compiled({ name: 'World' });    // 'Hello World!'

const list = template('<% _.forEach(people, function(name) { %><li><%= name %></li><% }); %>');
list({ people: ['fred', 'barney'] });    // '<li>fred</li><li>barney</li>'
```

---

## truncate

Truncates a string to a specified length.

**Function Signature:**
```typescript
function truncate(str: string, options?: TruncateOptions): string
```

**Description:** Truncates a string to the specified length, adding an omission string (default: '...') at the end if truncated.

**Example:**
```javascript
import { truncate } from 'es-toolkit';

truncate('hi-diddly-ho there, neighborino');    // 'hi-diddly-ho there, neighbo...'
truncate('hi-diddly-ho there, neighborino', { length: 24, separator: ' ' });  // 'hi-diddly-ho there,...'
truncate('hi-diddly-ho there, neighborino', { length: 24, omission: ' [...]' }); // 'hi-diddly-ho there [...]'
```

---

## unescape

Unescapes HTML entities in a string.

**Function Signature:**
```typescript
function unescape(str: string): string
```

**Description:** Converts HTML entities `&amp;`, `&lt;`, `&gt;`, `&quot;`, and `&#39;` back to their corresponding characters.

**Example:**
```javascript
import { unescape } from 'es-toolkit';

unescape('fred, barney, &amp; pebbles');    // 'fred, barney, & pebbles'
unescape('&lt;div&gt;');                     // '<div>'
```

---

## upperCase

Converts a string to uppercase with spaces between words.

**Function Signature:**
```typescript
function upperCase(str: string): string
```

**Description:** Converts a string to uppercase and separates words with spaces.

**Example:**
```javascript
import { upperCase } from 'es-toolkit';

upperCase('--foo-bar--');    // 'FOO BAR'
upperCase('fooBar');         // 'FOO BAR'
upperCase('__FOO_BAR__');    // 'FOO BAR'
```

---

## upperFirst

Converts the first character of a string to uppercase.

**Function Signature:**
```typescript
function upperFirst(str: string): string
```

**Description:** Changes only the first character of the string to uppercase, leaving the rest unchanged.

**Example:**
```javascript
import { upperFirst } from 'es-toolkit';

upperFirst('fred');     // 'Fred'
upperFirst('fred');     // 'Fred'
upperFirst('FRED');     // 'FRED'
```
