# Nodes without symbols are not supported

## Search terms

node, symbol, property

## Expected Behavior

It looks like in the following scenario `tsc` produces a node without a symbol
for `Source.SomeClass.someProp` ([[see AST
viewer]](https://ts-ast-viewer.com/#code/CYUwxgNghgTiAEYD2A7AzgF3gWwFzwG8AoeU+AZSWxAGFo018oUBPAGiIF8iiQAPAA5IYWDCwEJK1ACriEAXnhiJSAGY4AdFNr00GtFRAAFGEgEBuIA)):

`index.d.ts`
```ts
declare const m: {
    SomeClass: any,
}

export type SomeType = typeof m.SomeClass.someProp;
```

Type checker in TypeScript playground ([[see
here]](https://www.typescriptlang.org/play#code/CYUwxgNghgTiAEYD2A7AzgF3gWwFzwG8AoeU+AZSWxAGFo018oUBPAGiIF8iiQAPAA5IYWDCwEJK1ACriEAXnhiJSAGY4AdFNr00GtFRAAFGEgEBuIA))
can correctly deduce the type of `SomeType` to be `any`. I'm open to discussion
about what TypeDoc should do in this situation, but I think that it at least
should not crash.

## Actual Behavior

TypeDoc fails to generate documentation due to the following error:

```
Error: Expected a symbol for node with kind FirstNode at C:/dev/typedoc-symbol-bug-repro/index.d.ts:5
    at Context.expectSymbolAtLocation (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\context.js:102:19)
    at Object.convert (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\types.js:374:37)
    at Object.convertType (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\types.js:76:30)
    at Converter.convertType (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\converter.js:60:24)
    at Object.convertTypeAlias (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\symbols.js:131:45)
    at Object.convertSymbol (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\symbols.js:80:79)
    at Converter.convertExports (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\converter.js:181:23)
    at C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\converter.js:149:30
    at Array.forEach (<anonymous>)
    at Converter.compile (C:\dev\typedoc-symbol-bug-repro\node_modules\typedoc\dist\lib\converter\converter.js:147:17)
```

## Steps to reproduce the bug

1. Clone [the reproduction repository](https://github.com/ejuda/typedoc-symbol-bug-repro).
2. `npm ci`
3. `npm run docs`
4. The _Expected a symbol for node with kind FirstNode_ error should appear.

## Environment

-   Typedoc version: 0.21.5
-   TypeScript version: 4.3.5
-   Node.js version: 14.15.4
-   OS: Windows 10
