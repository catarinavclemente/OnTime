# SASS Compile

run the following in your emn\_theme directory:

npm install

This will install all dependencies (including Grunt) locally. After that, run Grunt using npx:

npx grunt theme

This will use the local version of Grunt and build your theme’s CSS.

```shellscript
npx grunt sass     # just compile
npx grunt theme    # compile + autoprefixer + stylelint + string-replace (recommended)
npx grunt          # full pipeline including watch mode
```

