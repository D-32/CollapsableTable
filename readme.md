# CollapsableTable [![Twitter](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](http://ctt.ec/310_J)

[![Carthage compatible](https://img.shields.io/badge/Carthage-Compatible-green.svg)](https://github.com/Carthage/Carthage)
[![Platform](https://img.shields.io/badge/Platform-iOS%208%2B-lightgrey.svg)]()

### Design

The collapsing mechanism can be installed in just a few minutes. But the creative design and styling of the UI, is at the mercy of the implementing developer. There are a handful of pre-built designs available within the demo target bundle named 'example'. However, after seeing them you may want to implement your own...

<details>
<summary>Creating your own is simple</summary>

Subclass RRNTableViewHeaderFooterView and override the following methods. The follwing implementation is how you might achieve the image rotation example shown by the demo .gif that is running within this readme.

```objective-c
-(void)openAnimated:(BOOL)animated {
    
    if (animated && !isRotating) {
        
        isRotating = YES;
        
        [UIView animateWithDuration:0.2 delay:0.0 options: UIViewAnimationOptionAllowUserInteraction |UIViewAnimationOptionCurveLinear animations:^{
            self.imageView.transform = CGAffineTransformIdentity;
        } completion:^(BOOL finished) {
            isRotating = NO;
        }];
        
    } else {
        [self.layer removeAllAnimations];
        self.imageView.transform = CGAffineTransformIdentity;
        isRotating = NO;
    }
}

-(void)closeAnimated:(BOOL)animated {
    
    if (animated && !isRotating) {
        
        isRotating = YES;
        
        [UIView animateWithDuration:0.2 delay:0.0 options: UIViewAnimationOptionAllowUserInteraction |UIViewAnimationOptionCurveLinear animations:^{
            self.imageView.transform = CGAffineTransformMakeRotation(DEGREES_TO_RADIANS(180.0f));
        } completion:^(BOOL finished) {
            isRotating = NO;
        }];
        
    } else {
        [self.layer removeAllAnimations];
        self.imageView.transform = CGAffineTransformMakeRotation(DEGREES_TO_RADIANS(180.0f));
        isRotating = NO;
    }
}

//Optional
-(void)updateTitle:(NSString *)title {
    self.titleLabel.text = title;
}
```
</details>

![](http://i.imgur.com/qLWgNxl.gif?1)

### Demo

Run each of the supplied XCode schemes.

### Installation

[Carthage](https://github.com/Carthage/Carthage) is a decentralized dependency manager that builds your dependencies and provides you with binary frameworks.

You can install Carthage with [Homebrew](http://brew.sh/) using the following command:

```bash
$ brew update
$ brew install carthage
```

To integrate `CollapsableTable.framework` into your Xcode project, using Carthage, specify it in your `Cartfile`:

```ogdl
github "rob-nash/collapsabletable"
```

Then run `carthage update` to fetch and build the binary file. To incorporate the file into your project, drag the binary into `embedded frameworks`. Once the binary is set as an embedded framework, any future versions can be installed by simply running carthage again, without the need to re-introduce the binary to your XCode project. You may want to specify a release and run carthage following `controlled cartifle amendments`, like so.

```ogdl
github "rob-nash/collapsabletable" == 1.0.0
```

Official binary releases are declared within the [Releases](https://github.com/rob-nash/CollapsableTable/releases) tab.

### Usage
1. Build a UITableView, either in code or from interface builder.
2. Subclass RRNTableViewHeaderFooterView and override all of it's public methods. Create a nib for this subclass with the same filename.
3. Subclass RRNCollapsableTableViewController and override all of it's public methods.

<details>
<summary>Something like this</summary>

```objective-c
-(UITableView *)collapsableTableView {
    return self.tableView;
}

-(NSString *)sectionHeaderNibName {
    return NSStringFromClass([CustomSectionHeaderView class]);
}

-(NSBundle *)sectionHeaderNibBundle {
    return [NSBundle bundleForClass:NSClassFromString([self sectionHeaderNibName])];
}

-(NSArray <RRNCollapsableTableViewSectionModelProtocol> *)menu {
    if (_menu == nil) {
        _menu = (NSArray <RRNCollapsableTableViewSectionModelProtocol> *)[FakeModelBuilder buildMenu];
    }
    return _menu;
}

-(NSArray <RRNCollapsableTableViewSectionModelProtocol> *)model {
    return self.menu;
}

-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
    return 44.0f;
}

-(CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section {
    return [CustomSectionHeaderView minimumHeight];
}

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    
//    id <RRNCollapsableTableViewSectionModelProtocol> mSection = self.menu[indexPath.section];
//    id item = mSection.items[indexPath.row];
    
    return [tableView dequeueReusableCellWithIdentifier:@"Cell"];
}
```
</details>

### Known issues
If you have > 8,000-10,000 rows in any given table view section, the frame rate drops to a noticable level.

# Licence
The MIT License (MIT)

Copyright (c) 2016 Rob Nash

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
