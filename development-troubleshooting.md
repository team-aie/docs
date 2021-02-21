## Development Troubleshooting

As developers, we all run into problems with our setup. This page collects these
problems and solutions to enrich our shared wisdom.

#### No files matching the pattern were found error on Windows

##### Error context:

OS: Windows 10 Electron: 9.x

![Error screenshot for No files matching the pattern were found error on
Windows](/assets/no-files-matching-the-pattern-01.png)

##### Root cause:

Comparing the command and the error message, it's evident that the file path is
cut off after the word `Senior`

whereas the original file path goes on: `...\Senior Project\...` . Therefore,
the conclusion is that the space in the path causes the command to break.

##### Solution:

Rename the folder to remove the space in the path.
