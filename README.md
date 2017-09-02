# CGd

This system package makes [*libgd*](https://libgd.github.io/)  available in Swift so that you can programatically create and edit image files. This package works with Swift 3.x and Swift 4.0.

Using this package enables you to perform image manipulation in a strictly procedural fashion because it simply makes the C functions in *libgd* available. If you would like to program in a Swiftier fashion, consider the Gd package (*to be announced soon*).

## Usage

Create a new Swift project:

    mkdir testcgd
    cd testcgd
    swift package init --type executable

Edit `Package.swift` to match the following:

```swift
import PackageDescription

let package = Package(
    name: "testcgd",
    dependencies: [
           .Package(url: "https://github.com/profburke/cgd.git", majorVersion: 1),
    ]
)
```

Then edit `Sources/main.swift` so that it reads as follows:

```swift
import CGd

let image = gdImageCreateTrueColor(400, 400)

let black = gdImageColorAllocate(image, 0, 0, 0)
let orange = gdImageColorAllocate(image, 255, 165, 0)
let string = UnsafeMutablePointer<UInt8>(mutating: "Pumpkin Carving")

gdImageFilledEllipse(image, 200, 200, 200, 200, orange)
gdImageString(image, gdFontGetGiant(), 20, 20, string, orange)

let outputFile = fopen("output.png", "wb")
defer { fclose(outputFile) }

gdImagePng(image, outputFile)
```

Now for building. 

We need to specify additional linker flags that the Swift build system needs to use. We could do so using `pkgConfig` in the package declaration. Or you can just add them to the build command as follows:

    swift build -Xlinker -L/usr/local/lib
    
Adjust as necessary if you have installed *libgd* to a different location. (*Of course, if you've done this, then the `module.modulemap` file probably won't work*.)


## License

This package is released under the MIT license, which is copied below.

Copyright (c) 2016-2017 Matthew M. Burke

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
