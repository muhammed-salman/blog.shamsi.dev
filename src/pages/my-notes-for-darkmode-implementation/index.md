---
title: My Notes for Dark Mode Implementation
date: '2021-09-21'
spoiler: Around a year ago I started implementing dark mode for one of my projects. I jotted down the points ready for your reference.
cta: 'ux,ui,design'
---

Around a year ago, I had started implementing dark mode for one of my projects. At that time, I had gone through a lots of reading to get into the nitty gritty details of the implementation. A day before yesterday one of my colleagues ask me to recollect thoughts on the same. Hence, I jotted down the points, ready for your reference and I hope that this make a useful post for you. Don't forget to share if you find it useful.

- ## Themes Toggling :
  - Custom class names like `.light .dark`

  - Custom attribute like `dark="true"`
  
  - Separate stylesheets
  
  - Server side initialization (cookies, database)

- ## Dark Mode Detection (Set at OS Level) :
  - CSS (`91.96%` supported at the time of publishing):

      `@media (prefers-color-scheme: dark)`
  
  - JS:

      `const prefersDarkScheme = window.matchMedia("(prefers-color-scheme: dark)");`
     
      **⚠️ Issue:** “flash of incorrect theme” (`FOIT`)

- ## Storing User Preference :
  - Local Storage: 
      - Suffers from `FOIT`
  
  - Cookies: 
      - Avoids `FOIT`
  
  - Database: 
      - Avoids `FOIT`

  <br/>
  
  > Note: I recommend using hybrid approach to avoid any potential FOIT & enhanced UX. The set of methods depends on your scenarios & requirements.

- ## Inform The Browser About Your Theme Modes (User Agent Styles) : 
  - Avoids slightest potential FOIT

  - HTML: (recommended)
    
      `<meta name="color-scheme" content="dark light">`

  - CSS: (`Support: 88.1%`, ⚠️ No Firefox support)
      ```
        :root {
          color-scheme: light dark;
        }
      ```

- ## Color Palette :
  - Lower saturation ( `200 tone` material UI recommends) ( `50-400` range is also ok )
  
  - A dark grey rather then black [ It is easier to see shadows on grey. Hence it leads to expression of wider range of color, elevation, and depth ] ( `#121212` material UI recommended)
  
  - Contrast ratio: `4.5:1 AA rating` (text, images, primary color ), `3:1 AAA rating` (large text)
  
  - Contrast ratio: `15.8:1` (dark surfaces)
  
  - Check accessibility: [Web Accessibility Evaluation Tool](https://wave.webaim.org/)
  
  - Limited use of brand color
  
  - Colors: Background (0dp elevation surface overlay),  Surface (with 1dp elevation surface overlay), Primary, Secondary, On background, On Surface, On Primary, On Secondary
  
  - `On` colors are basically font color, border color, etc which are used over background color

  - You can also have one or more variant of primary & secondary color as well  
  
  - Reserve bright colors for smaller surfaces

- ## Typography :
  - Font Weight should be less then lighter theme (White text look extra bold on black background)
  
  - De-saturated shade of pure white (reduce opacity)
  
  - Material UI Guidelines for text emphasis: 87%, 60%, 38% (opacity value starting from the highest emphasis text)

- ## Images :
  - Decrease brightness & increase contrast, so that it looks comfortable to eyes against the dark background
  
  - Typical values: 
      - Brightness: -20%
      - Contrast: +20%
  
  - Can use different pictures based on OS dark mode setting
      ```
      <picture>
          <!-- Use this image if OS setting is light mode or unset -->
          <source srcset="my-light-photo.jpg" media="(prefers-color-scheme: light) or (prefers-color-scheme: no-preference)">
          <!-- Use this image if OS setting is dark mode -->
          <source srcset="my-light-photo.png" media="(prefers-color-scheme: dark)">
      </picture>
      ```
     **⚠️ Caution:** Two separate files & does not account for toggling

- ## Elevation :
  - Elevation is inversely proportional to opacity (means lighter the surface color more is the elevation)
  
  - Shadows are replaced by elevation
  
  - However if you want to use shadow then use `dark shadow in dark mode`, do not invert the color of shadow used in light theme.
  
  - Dark Primary can be used with increased opacity for the nearest background element
    - Note the opacity of `5% to 20% for dark primary + 100% dark grey shade background` will create a substantial effect on background color.

- ## React Related :
  - Going for styled components?  
      - Use `ThemeProvider` component from styled component library
      - Examples: 
          - [A Dark Mode Toggle with React and ThemeProvider](https://css-tricks.com/a-dark-mode-toggle-with-react-and-themeprovider/) 
          - [Implementing Dark Mode In React Apps Using styled-components](https://www.smashingmagazine.com/2020/04/dark-mode-react-apps-styled-components/) 
  
  - Want to use CSS stylesheets? 
      - Go for custom attribute or custom class method

<br/>

---
  
🎉 🎉 🎉 Thats all folks! See below for further readings & references 🎉 🎉 🎉

---

- ## Further Reading & References :
  - [Material UI Dark Theme Guidelines](https://material.io/design/color/dark-theme.html)
  - [The Ultimate Guide on Designing a Dark Theme for your Android app](https://blog.prototypr.io/how-to-design-a-dark-theme-for-your-android-app-3daeb264637)
  - [A Complete Guide to Dark Mode on the Web](https://css-tricks.com/a-complete-guide-to-dark-mode-on-the-web/)
  - [The Expanding Gamut of Color on the Web](https://css-tricks.com/the-expanding-gamut-of-color-on-the-web/)
  - [Contrast (Minimum): Understanding SC 1.4.3](https://www.w3.org/TR/UNDERSTANDING-WCAG20/visual-audio-contrast-contrast.html)
  - [The Complete Guide to the Dark Mode Toggle](https://ryanfeigenbaum.com/dark-mode/)