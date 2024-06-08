# This project gathers some notes on 3d printing, largely with a focus on Klipper.
The notes are published to https://scopeuk.github.io/3dPrintingNotes/ by a github ci action when updates are made to the repository.   
This project absorbs my previous https://github.com/Scopeuk/LinuxForKlipperUsers notes project and has all of that content and more.   
  
The tooling used is mdBook and mdbook-pagetoc, this has been chosen as the published version is reasonably pretty and creating the inputs in markdown is not too painful.   
   
For development of notes it is recomended to install or provide mdBook and mdbook-pagetoc and then run `mdbook serve` this provides a live updating local webserver (http://localhost:3000) where changes to the notes files are immediately reflected in the preview.

My development is largely done on windows and pre compiled executables for both tools are avalaible from github, release links below   
https://github.com/rust-lang/mdBook/releases/   
https://github.com/slowsage/mdbook-pagetoc/releases/   
It is sufficient to extract the two executables to the root of the project directory, there are no further dependancies.   
On Linux or OSX based systems (with homebrew) these are likely avaliable from package management.   


Pull requests and discussion are welcome.   
