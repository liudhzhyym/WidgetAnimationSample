# WidgetAnimationSample

<img src="https://github.com/liudhzhyym/WidgetAnimationSample/blob/master/preview/preview.gif" alt="preview" width="400"/>

As we all know, it is very difficult to implement widget animation in WidgetKit, and almost all animation APIs are not available in WidgetKit.

There are probably several ways to implement WidgetKit:

### 1. One timelime per second

This method is the easiest to implement, but if you set a large number of timelines at one time, for example, you need to set 30*60=1800 timelines in 30 minutes, WidgetKit will be stuck, and the widget will be reloaded frequently, and the loading will fail, causing the widget to stop.

### 2. Using private API

This method can achieve continuous animation, but it may be rejected during appstore review, and it is more likely to cause the app to be removed from the shelf.


After studying the API interface of WidgetKit for a long time, I found a post on the Apple forum, https://developer.apple.com/forums/thread/720640 .

In this post, the Apple staff mentioned that widget animation can be realized with fonts. For this reason, I used an opportunity to seek code support from Apple and asked how to realize widget animation through font coordination. They did not give specific code implementation, just simple mentioned that animation can be realized through fonts and swiftui Text `.timer`.

After a long research, I finally found the solution:

### 1. Make a font file yourself

You can use tools such as Glyphs to create a ttf file, and use the ligature tool to combine two-digit combinations such as 00, 01, 02...59 into one character for display.

https://glyphsapp.com/learn/ligatures

Create a font file:

<img src="https://github.com/liudhzhyym/WidgetAnimationSample/blob/master/preview/font.jpg" alt="preview" width="800"/>

Then export it as ttf or otf file, import it to Xcode project.

### 2. Use Text timer in Widget and set custom font

```swift
struct widgetEntryView : View {
     var entry: Provider.Entry

     var body: some View {
         VStack() {
             Text(entry.date, style: .time)
             Text(Calendar. current. startOfDay(for: Date()), style: .timer)
                 .multilineTextAlignment(.center)
             Text(Calendar. current. startOfDay(for: Date()), style: .timer)
                 .font(Font.custom("test-font-Regular", size: 30))
                 .multilineTextAlignment(.center)
         }
     }
}
```


This project implements a simple blinking animation, you can make more complex fonts and achieve more complex animations. You are welcome to submit your own font files.
