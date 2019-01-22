# TypeDoc

Official documentation: http://typedoc.org/guides/doccomments/

Generating the website:

```bash
node_modules/typedoc/bin/typedoc --out ./docs --target ES5 --name "410 Warmup Project FrontEnd" --mode modules ./src
```

Comment style:

```typescript
/**
 * This is a description of a function called foo.
 * @param p1 This variable is a string ...
 * @param p2 This variable is a number...
 * @returns 
 */
function foo(p1: string, p2: number): number {
    return 20;
}
```

You can link to other classes, members or functions using double square brackets:

```

```

Examples:

- docs: http://peterdulworth.com/comp410/warmup/docs/
- global: http://peterdulworth.com/comp410/warmup/docs/modules/_components_tabs_tabbar_.html
- class: http://peterdulworth.com/comp410/warmup/docs/classes/_components_tabs_tabbar_.tabbar.html
- interface: http://peterdulworth.com/comp410/warmup/docs/interfaces/_components_tabs_tabbar_.tabbarstate.html

