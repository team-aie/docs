# Background

Vocaloid, a voice synthesizer, shook the music industry for over a decade.
Vocaloid leverages voicebanks, a collection of human voice samples and their
metadata, to generate human-like singing voices. Tools have been created to
enable average users to create their own voicebanks, but many of them lack
cross-OS and multi linguistic compatibility. To combat these shortcomings,
team-aie decided to create the next-generation tool, “aie”. Specifically, it is
going to address the issue of existing products with compatibility and usability
across a range of aspects by creating an Electron application and leveraging its
powerful capabilities out of the box. This document describes the existing
structure and design of aie and points out future work areas for the current
team.

# Version History

| Date       | Status      |
|------------|-------------|
| 2020-10-13 | First draft |

# UI design

The UI design is located at https://app.mockplus.cn/app/h7EZm9xha/design

Here are the screens coming from the design.

Note that in the dark mode, the fonts should be filled with colors (instead of
being transparent). The color is the same as the exterior lines.

### Welcome Page

![The mock of the welcome page in light
mode](assets/ui-design-welcome-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-welcome-page-dark.png)

### Create/select project

![The mock of the create project page in light
mode](assets/ui-design-create-select-project-page-light.png)

![The mock of the create project page in dark
mode](assets/ui-design-create-select-project-page-dark.png)

### Scale configuration

![The mock of the welcome page in light
mode](assets/ui-design-scale-configuration-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-scale-configuration-page-dark.png)

### Recording list preview (if “Show Detail” is clicked, will pull up from the bottom)

![The mock of the welcome page in light
mode](assets/ui-design-recording-list-preview-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-recording-list-preview-page-dark.png)

### Oto.ini preview (if “Switch To oto.ini” is clicked, will slide in from the right)

![The mock of the welcome page in light
mode](assets/ui-design-otoini-preview-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-otoini-preview-page-dark.png)

### Voice.dvcfg preview (if “Switch To voice.dvcfg” is clicked, will slide in from the right)

![The mock of the welcome page in light
mode](assets/ui-design-voicedvcfg-preview-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-voicedvcfg-preview-page-dark.png)

### Built-in recording list usage agreement (ToC) (If exists, if new project)

![The mock of the welcome page in light
mode](assets/ui-design-usage-agreement-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-usage-agreement-page-dark.png)

### Recording screen (correction: should display a gear button in the top right corner in this screen, not the scale configuration screen)

![The mock of the welcome page in light
mode](assets/ui-design-recording-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-recording-page-dark.png)

### Audio settings (if click on the gear button, or display when entering the project after the user agrees to the ToC)

![The mock of the welcome page in light
mode](assets/ui-design-audio-settings-page-light.png)

![The mock of the welcome page in dark
mode](assets/ui-design-audio-settings-page-dark.png)

# Detailed design

## Electron integration/Main process

The main process entry point is located at `src/main/index.ts` . It mostly
follows the official Electron boilerplate, which implements good default
behavior for cross-OS app launching and termination. The addition to the main
process launch code is best summarized as follows.

### Single-Instance Lock

The current implementation uses a pattern to ensure that when the user tries to
launch another instance of the application (such as launching it by
double-clicking the desktop icon):

1. No second instances are launched
1. If the first instance is minimized, this will also bring up that instance to
   the front

The first point is implemented with the following:

``` typescript
const isFirstInstance = app.requestSingleInstanceLock();

if (!isFirstInstance) {
  app.quit();
}
```

Whereas the second point is implemented with an event handler:

``` typescript
app.on('second-instance', () => {
  if (mainWindow) {
    if (mainWindow.isMinimized()) {
      mainWindow.restore();
    }
    mainWindow.focus();
  }
});
```

### Custom Menubar

For aesthetics and consistency with the design mock, the menu bar is hidden on
Windows and Linux by adding `Menu.setApplicationMenu(null);` . However, on
macOS, the menubar is not attached to the window but usually displayed farther
at the top of the screen. Therefore, it is not necessary to remove the menubar
altogether. Instead, the current implementation adds custom options to the menu
instead. This is achieved by creating a template object and calling
`Menu.buildFromTemplate(template)` to build the menubar and set it.

### Auto Updater

Electron Builder supports auto-updating [out of the
box](https://www.electron.build/auto-update). However, this application
currently doesn’t satisfy all of the requirements to enable that for macOS
users. Notably, in order to satisfy the requirements, the team needs to set up
code signing (notarization) of the macOS app. Because this process requires a
paid Apple Developer Account, this is not yet implemented. Instead, the current
implementation creates a wrapper of the Electron Builder auto-updater to
customize the behavior, including disabling it for macOS users, and displaying a
notification of the new version. When the user clicks on the notification, it
will open a link to let users download the new version themselves.

## Building and Releasing

The application uses Electron Webpack to build the app. However, there are
several customizations done to the Webpack configuration using customized
`webpack.something.config.js` files. Releasing is done using Semantic Release
with Github Actions. To allow for accurate calculation of next version numbers,
during development, the team must keep in mind the format of the commit message
that Semantic Release can understand. Semantic Release is also configured to
upload the packaged app to GitHub to release the next version of the app. Later,
auto-updater will query GitHub API to find this new version and apply updates.

### License Acknowledgement

The application provides a link to display the open-source licenses that the
project has depended upon. This is done using the
[WebpackLicensePlugin](https://github.com/codepunkt/webpack-license-plugin). To
support its usage, a nearly identical Webpack config is created with Webpack
hooks added to collect license information from WebpackLicensePlugin. Then, at
the end of the Webpack execution, it will write the information to
`src/renderer/licenses.ts` . This file is first overwritten with
`src/renderer/licenses.ts.base` , and the script will replace
`__LICENSE_CONTENT_HERE__` with serialized license information. Later on, this
file will be compiled with other TSX files to generate a table of open-source
licenses.

## Bundle Splitting of CSS Assets for Locales

A section is added to the renderer config to create separate CSS assets for each
of the locales. Later on, this will be read into the app via StyleSwitcher. More
details are in the Internationalization section.

## Internationalization

Internationalization is done on two levels: language/locale and font. When the
user updates their locale in the application, it not only loads the
corresponding translations but also loads the font package associated with that
locale. Among certain languages, one character might have the same Unicode
encoding, but they are written differently, which makes changing fonts
necessary. To make finding locale-specific translations and font configurations
easier, they are located at `src/locales/<locale ISO code>` . The translation
file is called “translations.json”, which contains a dictionary that maps from
translation key (usually the corresponding entry in English) to the translation
value (which is the translated expression). Because fonts are specified in CSS
via the `font-family` value, inside each locale folder, there is also an
“index.scss” file that specifies different fonts in each locale. During build,
each locale will generate its own CSS file, and can be loaded later with Webpack
loaders.

Detection, memorization, and implementation of locale switching are implemented
via `react-i18next` . It is a React-friendly `i18next` extension, meaning it
also depends on the `i18next` package as well. It has several detectors to
detect user locale when the app launches, and so far only navigator and local
storage ones are enabled. Navigator provides information regarding the user OS’s
locale settings, which should be a good default if the user launches the app for
the first time. Local storage is configured to store the user selection of the
locale whenever the selection is updated. Besides detection, `i18next` exposes
APIs to set the locale directly, which is used by LocaleSelector at
`src/renderer/components/locale-selector/index.tsx` .

To inform the rest of the application of the locale, a LocaleContext is created
at `src/renderer/contexts/locale-context.ts` and is provided at the root level
to the rest of the app. It is used by StyleSwitcher  at
`src/renderer/components/style-switcher/index.tsx` to swap for the
locale-specific CSS file.

## Accessibility

Currently, the application lacks provisions for accessibility in the custom
components. To satisfy the requirement that the entire application can be used
with only a keyboard, there is at least an action item of adding a `tabindex=0`
attribute to each of these components. However, as clearer requirements
regarding accessibility appear, there might be a need for further enhancement.
It is suggested that the team implement a library of custom components, such as
image-based buttons, that conform to the accessibility standards, and use these
components throughout the application.

## Project/Application State Saving and Loading

The app needs to ensure that its project can be copied-and-pasted to other
locations or computers and can still be read and used. This would most likely
result in a project file solution. However, this solution is currently missing.
The choice here will be relevant to the implementation of the project creation,
selection, and closure UI flow.

The app itself remembers the past projects and the last accessed timestamp
locally using an IndexedDB-backed [Dexie](https://dexie.org/) instance.

It is a requirement that the app remains a native feeling in that, for macOS
users, when they close the window and launch the new one, the application must
not forget where it was at - similar to the experience of minimizing and then
bringing to the front on Windows. Current solution is to record state variables
to local storage using `useLocalStorage` from `react-use` rather than `useState`
directly. However, discretion is needed when the state variable is concerned
with audio input devices, which may or may not stay the same when the app wakes
up.

## Web Animation

There are a few screen transitions that require animation. This poses particular
challenges on page routing: without animation, there is one page that is
displayed at a time; with animation, there are times when the current page and
the next page need to be displayed at the same time, so the routing logic may
need a redesign. Currently, there are a limited set of “states” pre-defined, and
each state corresponds to a page component.

The implementation of such animation is also unknown. It may be worthwhile to
consider using “react-transition-group” and other libraries to assist with this.

## Global Key Event Handler

On the recording page, the app needs to listen for the keyboard event globally
in the app. This is currently implemented as a service to register and remove
callbacks  at `src/renderer/services/key-event-handler-registry` , but this does
not encourage dividing key event handlers into ones that deal with specific keys
and event types, rather a giant handler that uses a giant switch statement to
branch off them. This type of handler will not scale well in the future.

## Audio I/O and Manipulation

Media I/O is currently centralized in `src/renderer/services/media` . It knows
how to request for microphone permission, record microphone input, connect audio
graphs, and so on. It is too big to test easily, and also makes testing other
components more difficult. This needs to be broken down into smaller pieces.

## Audio Visualization

Currently, the visualization
`src/renderer/components/recording-page/recording-visualization.tsx` is done
after the recording is complete. This is inconsistent with the intended design
(which is real-time). Also, due to the limitation of the library used
([wavesurfer.js +
Spectrogram](https://wavesurfer-js.org/example/spectrogram/index.html)), it is
implemented as a routine check-then-update loop running in an interval, which is
more costly in terms of performance.

### Design Plan for Rendering Live Visualizations
Method Used:
- waveform.js Microphone Plugin

Related Existing Code:
- Add in types/ a wavesurfer.microphone.d.ts file
  - Ensures that the plugin imports correctly (similar to types/wavesurfer.spectrogram.d.ts)
  - Export a MicrophonePluginParams interface
  - Based on top comment on https://wavesurfer-js.org/doc/file/src/plugin/microphone.js.html
  - Export a MicrophonePlugin class with a static create function that takes in an input of type MicrophonePluginParams
    - Again, based on this source code https://wavesurfer-js.org/doc/file/src/plugin/microphone.js.html
- In recording-page/recording-visualization.tsx:
  - Add acquireAudioInputStream to the import statement from ../../utils
  - `import { useAudioInputOutputDevices } from '../settings-page/hooks';`
    - This is the subscription to the audio devices, the current input device id is used to acquire the used audio input stream on the microphone plugin
  - `const [, , audioInputDeviceId, , ,] = useAudioInputOutputDevices();`
    - Get the audio input device id, we don’t care about the other variables that useAudioInputOutputDevices returns
  - `MicrophonePlugin.create({}),`
    - Add the microphone plugin to the existing wavesurfer instance’s plugin array
  - In the useEffect on [filePath, prevState, state]
    - Add audioInputDeviceId to the dependencies to run the useEffect on
    - Add `waveSurfer.microphone.pause();` in the not recording if before you read from the .wav file to make sure that the live visualizations are stopped and the instance is ready for the static visualizations
    - Add an else if for when the application is in a recording state
      - If the mic plugin is not active, call the acquireAudioInputStream to get the media stream object to pass into wavesurfer.microphone.gotStream which initializes the mic with the promised media stream
      - Else just call `waveSurfer.microphone.play();`

Design Detail:
- Use the waveform.js microphone plugin to do the live waveform.
- Most of the edit would be in renderer\components\recording-page\recording-visualization.tsx

### Design Plan for Rendering Static Visualizations
Method Used:
- waveform.js

Related Existing Code:
- In recording-page/recording-visualization.tsx:
  - Add a FileMonitor item and have it watch when files are added, unlinked, and changed to the project folder
  - Subscribe to that filemonitor item’s getSubject() function
    - Limit the events to ones that deal with just the current recitem’s wav file
    - On unlink empty the visualizations
    - On add reload the visualizations with the new file

Design Detail:
- If the recording item has a related .wav file
  - Show a waveform and spectrogram from the related .wav file
- If the recording item does not has a related .wav file
  - Show no spectrogram or waveform - would reuse the current code for this

### Design Plan for AutoZoom on Visualizations
Method Used:
- waveform.js

Related Existing Code:

Design Detail:
- Not going to do anything special, the automatic zooming on wavesurfer.js is sufficient to meet client needs
