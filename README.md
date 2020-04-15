<p align="center">
	<img alt="Fastis" src="Documentation/top_screen.jpg" srcset="top_screen@2x.jpg 2x">
</p>

[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Swift](https://img.shields.io/badge/Swift-5-green.svg?style=flat)](https://swift.org)
[![Xcode](https://img.shields.io/badge/Xcode-11-blue.svg?style=flat)](https://developer.apple.com/xcode)

Fastis is a fully customizable UI component for picking dates and ranges created using [JTAppleCalendar](https://github.com/patchthecode/JTAppleCalendar) library.

- [Requirements](#requirements)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
	- [Quick Start](#quick-start)
	- [Single and range modes](#single-and-range-modes)
	- [Configuration](#configuration)
	- [Shortcuts](#shortcuts)
	- [Customization](#customization)
- [Credits](#credits)
- [License](#license)

## Requirements

- iOS 11.0+
- Xcode 11.0+
- Swift 5.0+

## Features

- [x] Flexible customization
- [x] Shortcuts for dates and ranges
- [x] Single date and date range modes

## Instalation

### Carthage

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate Fastis into your Xcode project using Carthage, specify it in your `Cartfile`:

```ogdl
github "retailcrm/Fastis" ~> 1.0.0
```

Run `carthage update` to build the framework and drag the built `Fastis.framework` into your Xcode project.

### Swift Package Manager

The [Swift Package Manager](https://swift.org/package-manager/) is a tool for automating the distribution of Swift code and is integrated into the `swift` compiler.

Once you have your Swift package set up, adding Fastis as a dependency is as easy as adding it to the `dependencies` value of your `Package.swift`.

```swift
dependencies: [
    .package(url: "https://github.com/retailcrm/Fastis.git", .upToNextMajor(from: "1.0.0"))
]
```

### Manually

If you prefer not to use either of the aforementioned dependency managers, you can integrate Fastis into your project manually.


## Usage

### Quick Start

```swift
import Fastis

class MyViewController: UIViewController {

   func chooseDate() {
        let fastisController = FastisController(mode: .range)
        fastisController.title = "Choose range"
        fastisController.maximumDate = Date()
        fastisController.allowsToChooseNilDate = true
        fastisController.shortcuts = [.today, .lastWeek]
        fastisController.doneHandler = { resultRange in
            ...
        }
        fastisController.present(above: self)
    }

}
```

### Single and range modes

If you want to get a single date you have to use `Date` type:

```swift
let fastisController = FastisController(mode: .single)
fastisController.initialValue = Date()
fastisController.doneHandler = { resultDate in
	print(resultDate) // resultDate is Date
}

```

If you want to get a date range you have to use `FastisRange` type:

```swift
let fastisController = FastisController(mode: .range)
fastisController.initialValue = FastisRange(from: Date(), to: Date()) // or .from(Date(), to: Date())
fastisController.doneHandler = { resultRange in
	print(resultRange) // resultDate is FastisRange
}
```

### Configuration

FastisController have the following default configuration parameters:

```swift
var shortcuts: [FastisShortcut<Value>] = []
var allowsToChooseNilDate: Bool = false
var dismissHandler: (() -> Void)? = nil
var doneHandler: ((Value?) -> Void)? = nil
var initialValue: Value? = nil
var minimumDate: Date? = nil
var maximumDate: Date? = nil
var selectMonthOnHeaderTap: Bool = true
```

- `shortcuts `- Shortcuts array. Default value is `[]`. See [Shortcuts](#shortcuts) section
- `allowsToChooseNilDate `- Allow to choose `nil` date. If you set `true` done button will be wlways enabled. Default value is `false`.
- `dismissHandler `- The block to execute after the dismissal finishes. Default value is `nil`.
- `doneHandler `- The block to execute after "Done" button will be tapped. Default value is `nil`.
- `initialValue `- And initial value which will be selected bu default. Default value is `nil`.
- `minimumDate `-  Minimal selection date. Dates less then current will be markes as unavailable. Default value is `nil`.
- `maximumDate `- Maximum selection date. Dates greather then current will be markes as unavailable. Default value is `nil`.
- `selectMonthOnHeaderTap ` (Only for `.range` mode) - Set this variable to `true` if you want to allow select date ranges by tapping on months. Default value is `true`.

### Shortcuts

Using shortcuts allows you to quick select prepared dates or date ranges.
By default `.shortcuts` is empty. If you don't provide any shortcuts the bottom container will be hidden.

In Fasis available some prepared shortcuts for each mode:

- For **`.single`**: `.today`, `.tomorrow`, `.yesterday`
- For **`.range`**: `.today`, `.lastWeek`, `.lastMonth`

Also you can create your own shortcut:     

```swift
var customShortcut = FastisShortcut(name: "Today") {
	let now = Date()
	return FastisRange(from: now.startOfDay(), to: now.endOfDay())
}
fastisController.shortcuts = [customShortcut, .lastWeek]
```

### Customization

Fastis can be customized global or local. `FastisConfig` have some sections:

- `controller` - base view controller (`cancelButtonTitle`, `doneButtonTitle`, etc.)
- `monthHeader` - month titles
- `dayCell` - day cells (selection parameters, font, etc.)
- `weekView` - top header view with week day names
- `currentValueView` - current value view appereance (clear button, date format, etc.)
- `shortcutContainerView` - bottom view with shortcuts
- `shortcutItemView` - shortcut item in the bottom view

To cutomize all Fastis controllers in your app use `FastisConfig.default`:

```swift
FastisConfig.default.monthHeader.labelColor = .red
```

To customize special FastisController instance:

```swift
var customConfig = FastisConfig.default
customConfig.controller.dayCell.dateLabelColor = .blue
let fastisController = FastisController(mode: .range, config: customConfig)
```

## Credits

- Ilya Kharlamov ([@ilia3546](https://github.com/ilia3546))

## License

Fastis is released under the MIT license. See LICENSE for details.