# quartz_depthifier
Adds depth to anything with an [AST selector](https://github.com/syntax-tree/mdast) on your Quartz site.

## Installation 
1. Navigate to ./quartz/plugins/transformers and drop the depthifier.ts file in there.
2. Add the following line to the index.ts *IN THAT SAME FOLDER*, not the index.ts outside of it.
   ```ts
   export { Depthifier } from "./depthifier"
3. Go back to the root of your project and go to ./quartz.config.ts, and in the transformer plugin section add Depthifier and the selectors you want to find the depth of.
   ```ts
     plugins: {
       transformers: [
         ...,
         ...,
         Plugin.Depthifier({ selectors: ["strong", "paragraph"] })
       ],
       ...
     }

## Themeing
What this plugin does is add a custom "depth" value to your nodes of choice for themeing with S/CSS. Here is an example with the strong element.

Before:
   ```html
   <strong>the distance from Earth to the Sun,</strong>
   ```

After:
   ```html
   <strong depth="4">the distance from Earth to the Sun,</strong>
   ```

This is meant mostly to be used to theme thing based off of the header depth they are. For example, to theme strong text to be the color of its parent header (and clean that whole shebang up a bit), I have the following setup:

```scss
$colors: (
  "h1": $depth1,
  "h2": $depth2,
  "h3": $depth3,
  "h4": $depth4,
  "h5": $depth5,
  "h6": $depth6
);

@each $header, $color in $colors {
  #{$header} {
    color: $color;

    & span.math.math-inline span.katex {
      color: $color!important;
    }
  }
  
  strong[depth="#{str-slice($header, 2)}"] {
    color: $color;

    & span.math.math-inline span.katex {
      color: $color!important;
    }
  }
}
