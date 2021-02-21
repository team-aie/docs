## Resources

This page hosts resources for team members to familiarize themselves on related
topics.

### Background

* YouTube Video: [So, you wanna get into VOCALOID - A Beginner's Guide by
  JOEZCafe (Basics, Music and Software
  Tips)](https://www.youtube.com/watch?v=y5pPALER7WU)
    - It’s actually quite comprehensive. Touches on voice synthesizers as a
      whole, the derivative work/industry based off of it, and various other
      synthesizers (not just Vocaloid)


      The timestamps in the description help to navigate between different
      sections.

* YouTube Video: [【UTAU tutorial #3】 Voicebank & UST
  types](https://www.youtube.com/watch?v=TPVsHdqpCvc)
    - Even though it’s a UTAU tutorial, it offers a good overview of voicebank
      types. Note that since UTAU is a Japanese software, many songs (and the
      ecosystem around it) assumes Japanese.


      Terms like “CVVC” will occasionally show up in this project, so might as
      well have an idea of what they are.

* YouTube Video: [VCCV English UTAU Tutorial -
  OREMO](https://www.youtube.com/watch?v=3IeipkpO7KU)
    - See Oremo in action. How to use it to record voice samples (note it uses
      concepts from the last video)
* YouTube Video: [VCCV English UTAU Tutorial -
  SETPARAM](https://www.youtube.com/watch?v=t_epqCMN7bQ)
    - Provides lesser-needed knowledge about the labeling of voice samples
      (after recording them in Oremo).


      The proposed audio processing feature is to generate this timing
      information automatically.

      Some people may call it “oto-ing”, where “oto” is “Sound” in Japanese

### DevOps

* Topic: GitHub Actions/Workflow
    - [GitHub Actions Documentation](https://docs.github.com/en/actions)
    - Currently, this is where CI/CD is implemented.
* Topic: GitHub Projects
    - [Managing project
      boards](https://docs.github.com/en/github/managing-your-work-on-github/managing-project-boards)
* Topic: Angular Commit Message Format
    - [Git Commit
      Guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)
    - This is the format used by `semantic-release` to automatically bump the
      release version.
* Topic: Git and GitHub Setup
    - [Setting your commit email address in
      Git](https://docs.github.com/en/github/setting-up-and-managing-your-github-user-account/setting-your-commit-email-address#setting-your-commit-email-address-in-git)
    - [Generating a new SSH key and adding it to the
      ssh-agent](https://docs.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
    - [Configuring Git to handle line endings - may or may not need to use the
      suggested setting. Depends on linter rules and IDE
      settings](https://docs.github.com/en/github/using-git/configuring-git-to-handle-line-endings)
    - Your email address must be the same as your GitHub email. Or, if you set
      your email to be private, use the GitHub-provided email instead.


      Using SSH to access GitHub can save you time typing passwords. And, if you
      ever SSH into machines, you can upload your SSH public to the machine and
      you will never need to type your password to SSH into that machine too.

### Tooling

* [Code Blocks - G Suite
  Marketplace](https://gsuite.google.com/marketplace/app/code_blocks/100740430168)
    - This is a Google Doc Add-On that enables syntax-highlight code blocks in
      Google Doc

### Libraries, Frameworks, etc.

* Topic: TypeScript
    - [The TypeScript
      Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
* Topic: React Hooks
    - [Hooks at a Glance](https://reactjs.org/docs/hooks-overview.html)
    - Most people are familiar with React Class Components but React Hooks is a
      new and simplistic way to define components.

      Most of the time, a functional component with hooks will suffice.

      The project currently uses [ `react-use`
      ](https://github.com/streamich/react-use) and custom hooks to organize the
      code.

* Topic: Accessibility (a11y)
    - [Web Fundamentals -
      Accessibility](https://developers.google.com/web/fundamentals/accessibility)
* Topic: Internationalization (i18n)
    - [i18next/react-i18next](https://github.com/i18next/react-i18next)
* Topic: CSS/SCSS
    - [Sass Basics](https://sass-lang.com/guide)
    - [React Bootstrap](https://react-bootstrap.github.io/)
* Topic: Electron and its ecosystem
    - [Electron Documentation](https://www.electronjs.org/docs) |
      [remote](https://www.electronjs.org/docs/api/remote)
    - [Electron Builder](https://www.electron.build/) | [Auto
      Update](https://www.electron.build/auto-update)
    - [Electron Webpack](https://webpack.electron.build/) | [Project
      Structure](https://webpack.electron.build/project-structure) | [Modifying
      Webpack
      Configurations](https://webpack.electron.build/modifying-webpack-configurations)
* Topic: MediaStream, MediaDevice, and AudioElement API
    - [MediaStream Recording
      API](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream_Recording_API)
    - [MediaDevices.getUserMedia()](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia)
    - [HTMLMediaElement.setSinkId()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/setSinkId)
* Topic: Web Audio API
    - [Using the Web Audio
      API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Using_Web_Audio_API)
* Topic: Storage API
    - [Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
    - [IndexedDB
      API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API)
    - [Dexie.js - Getting
      Started](https://dexie.org/docs/Tutorial/Getting-started)
* Topic: Animation
    - [React Transition
      Group](https://reactcommunity.org/react-transition-group/)
* Topic: Web Worker API
    - [Service Worker
      API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
    - [Web Workers
      API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API)
