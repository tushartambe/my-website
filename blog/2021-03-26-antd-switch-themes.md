---
slug: antd-switch-themes
title: Switching between themes in antd
author: Tushar Tambe
author_title: Application Developer @ThoughtWorks
author_url: https://github.com/tushartambe
author_image_url: https://avatars.githubusercontent.com/u/44019295?s=460&u=5029ee2f0952a1ecf522850e1d1b8c6e2f0695f7&v=4
tags: [css, html, react, antd, themes]
---

<br/>
You must have come across websites where you can switch between themes. You also want to create one in your react app with very less effort?
This blog cum tutorial will solve your problem.

<!--truncate-->

### Little about `antd`

Antd also known as **Ant Design** is an open source component library for React. Antd provides very large set of ready made components with built in styling. You can explore and play with it [here](https://ant.design/components/overview/).

Antd also supports customizing themes using [less](http://lesscss.org/) variables. We are not going to customize any theme, instead we will be using predefined light and dark theme in antd.

### Creating new react app

Create new react boilerplate app with help of `create-react-app`

``` bash
npx create-react-app antd-react-switch-themes
```

Start the app

``` bash
antd-react-switch-themes && npm start
```

### Add antd

Now lets add antd as dependency in out react project.

``` bash
npm i antd
```

### Importing antd theme styles

We need to create less stylesheets and import antd styles in it.
So lets create our two `.less` files under `src/themes/` directory.
 - `dark-theme.js`

 - `light-theme.js`

We will use antd defined theme colors, for that import antd theme color variables in our file.

**`light-theme.less`**

``` less
@import '~antd/lib/style/color/colorPalette.less';
@import '~antd/dist/antd.less';
@import '~antd/lib/style/themes/default.less'; //will import light theme colors defined by antd

@primary-color: #06d6a0; //this will override primary color for light theme defined by antd
```

**`dark-theme.less`**

``` less
@import '~antd/lib/style/color/colorPalette.less';
@import '~antd/dist/antd.less';
@import '~antd/lib/style/themes/dark.less'; //will import dark theme colors defined by antd
```

you can change default antd colors by overriding there variables in our theme files.
Check antd color variables:
 - [Default(Light) theme](https://github.com/ant-design/ant-design/blob/master/components/style/themes/default.less)
 - [Dark theme](https://github.com/ant-design/ant-design/blob/master/components/style/themes/dark.less)

### Compiling our theme files

Now we have to compile less variables to css and add them to public folder so that can be injected in client.

To compile less files into css we will use [gulp task runner](https://gulpjs.com/docs/en/getting-started/quick-start)
lets add gulp and other necessary dependencies.

``` bash
npm i gulp gulp-less gulp-postcss gulp-debug gulp-csso autoprefixer less-plugin-npm-import --save-dev
```

now create `gulpfile.js` at root level of project.

``` jsx
const gulp = require('gulp')
const gulpless = require('gulp-less')
const postcss = require('gulp-postcss')
const debug = require('gulp-debug')
const csso = require('gulp-csso')
const autoprefixer = require('autoprefixer')
const NpmImportPlugin = require('less-plugin-npm-import')

gulp.task('less', function () {
  const plugins = [autoprefixer()]

  return gulp
    .src('src/themes/*-theme.less')
    .pipe(debug({title: 'Less files:'}))
    .pipe(
      gulpless({
        javascriptEnabled: true,
        plugins: [new NpmImportPlugin({prefix: '~'})],
      }),
    )
    .pipe(postcss(plugins))
    .pipe(
      csso({
        debug: true,
      }),
    )
    .pipe(gulp.dest('./public'))
})

```

to compile less to css, lets run following command 

``` bash
npx gulp less
```

now all less files are compiled to css and you can see it under `public/` directory.

### Switching themes

Now we have done all this work of loading antd themes and compiling them to css but how to use it?

To swap between pre-fetched css styling we will use [React CSS Theme Switcher](https://github.com/JoseRFelix/react-css-theme-switcher)

Add `React CSS Theme Switcher` as dependency

``` bash
npm i react-css-theme-switcher
```

now wrap our `App` component with `ThemeSwitcherProvider` in `index.js` as following with defined themes.
 `index.js`

``` jsx
import React from 'react';
import { ThemeSwitcherProvider } from "react-css-theme-switcher";
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

const themes = {
  dark: `${process.env.PUBLIC_URL}/dark-theme.css`,
  light: `${process.env.PUBLIC_URL}/light-theme.css`,
};

ReactDOM.render(
  <React.StrictMode>
    <ThemeSwitcherProvider themeMap={themes} defaultTheme="light">
      <App />
    </ThemeSwitcherProvider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

Now switch between themes using `useThemeSwitcher` of `react-css-theme-switcher`

``` jsx
import { Switch } from 'antd';
import './App.css';
import { useThemeSwitcher } from "react-css-theme-switcher";
import { useState } from 'react';

function App() {
  const [isDarkMode, setIsDarkMode] = useState(false);
  const { switcher, themes } = useThemeSwitcher();

  const switchTheme = (isDarkMode) => {
    setIsDarkMode(isDarkMode);
    switcher({ theme: isDarkMode ? themes.dark : themes.light });
  };

  return (
    <div className="App">
      <Switch
        checkedChildren={"Dark Theme"}
        unCheckedChildren={"Light Theme"}
        checked={isDarkMode}
        onChange={switchTheme}
      />
    </div>
  );
}

export default App;
```

:::tip

Code is available on [github](https://github.com/tushartambe/antd-react-switch-themes).

:::
