---
title: Upside-down Lists
date: 2023-01-06 17:43:00
categories: [Devlog]
tags: [noot,swiftui,swift]
---
Carrying on from the Noot development thread we have started, one of things that I wanted to try was for new notes appear at the bottom of the list in the same way that the Messages app has it's entries bottom-up.

This sounds like a standard list with the data reverse-ordered. But you need to take into account a few things:

* That when the list is opened, you need to be on the bottom of the list
* Content should animate in from the bottom
* The natural scroll-to-top behaviour should actually be a scroll to the bottom.

Might be a bit easier with an example, this is what I've got going on in Noot:

![Noot List](/assets/img/2023-01-06-noot-list.gif){: width="320" }

I really wanted to do this without resorting to any UIKit magic.

It's actually pretty straightforward to do using the twin powers of rotation and scale.

List start off with a simple SwiftUI List with some content:

```swift
import SwiftUI

struct UpsideDownList: View {

    let items = (0...10).map{$0}

    var body: some View {
        ScrollView {
            LazyVStack {
                ForEach(items, id:\.self) { item in
                    Text("Item number \(item)")
                        .frame(maxWidth: .infinity)
                        .padding()
                        .background {
                            RoundedRectangle(cornerRadius: 12)
                                .fill(.secondary)
                        }
                        .padding()

                }
            }
        }
    }
}

```
Note: I use a `ScrollView` and `LazyVStack` here instead of a `List` because I find Lists problematic in so many ways in SwiftUI......

Anyway, this results in:

![Plain List](/assets/img/2023-01-06-plain-list.png){: width="320" }

So, let's first try rotating this whole thing by 180° and see what happens. We do this by applying a `rotationEffect` of `pi` radians:

```swift
var body: some View {
    ScrollView {
        LazyVStack {
            ForEach(items, id:\.self) { item in
                Text("Item number \(item)")
                    .frame(maxWidth: .infinity)
                    .padding()
                    .background {
                        RoundedRectangle(cornerRadius: 12)
                            .fill(.secondary)
                    }
                    .padding()

            }
        }
    }
    .rotationEffect(.radians(Double.pi))
}
```

![Plain List Rotated](/assets/img/2023-01-06-plain-list-rotated.gif){: width="320" }

Ok, the list is definitely upside-down, but so is the content, and you'll also notice that the scroll bar is now on the opposite side. What we really want to do is flip this thing across the x-axis.

To do this, we will apply a `scaleEffect` of `-1` in `x` and use the `.center` as the anchor for the rotation. (I've seen this done before in video game development to make a sprite face the opposite direction.)

Adding this gives us:

```swift
var body: some View {
    ScrollView {
        LazyVStack {
            ForEach(items, id:\.self) { item in
                Text("Item number \(item)")
                    .frame(maxWidth: .infinity)
                    .padding()
                    .background {
                        RoundedRectangle(cornerRadius: 12)
                            .fill(.secondary)
                    }
                    .padding()

            }
        }
    }
    .rotationEffect(.radians(Double.pi))
    .scaleEffect(x: -1, y: 1, anchor: .center)
}
```

![List rotated and flipped](/assets/img/2023-01-06-plain-list-rotated-flipped.gif){: width="320" }

So, we're done. The list is flipped and the first item is now at the bottom. Great!!!

Time for coffee...

Ok, you may have noticed a slight issue with the content. Unless you really want to strain your neck reading we need to fix this.

Fortunately, all we have to do is exactly the same thing to each item in the list - rotate and scale:

```swift
var body: some View {
    ScrollView {
        LazyVStack {
            ForEach(items, id:\.self) { item in
                Text("Item number \(item)")
                    .frame(maxWidth: .infinity)
                    .padding()
                    .background {
                        RoundedRectangle(cornerRadius: 12)
                            .fill(.secondary)
                    }
                    .padding()
                    .rotationEffect(.radians(Double.pi))
                    .scaleEffect(x: -1, y: 1, anchor: .center)

            }
        }
    }
    .rotationEffect(.radians(Double.pi))
    .scaleEffect(x: -1, y: 1, anchor: .center)
}
```

Look at that beauty:

![Flipped List and Content](/assets/img/2023-01-06-plain-list-rotated-flipped-content.gif){: width="320" }

We are using the the same code twice here. Let's not do that, lets create a custom `ViewModifier` and an extension on `View`, so we can re-use it across any view we like.

Here is the full code:

```swift
import SwiftUI

struct UpsideDownList: View {

    let items = (0...10).map{$0}

    var body: some View {
        ScrollView {
            LazyVStack {
                ForEach(items, id:\.self) { item in
                    Text("Item number \(item)")
                        .frame(maxWidth: .infinity)
                        .padding()
                        .background {
                            RoundedRectangle(cornerRadius: 12)
                                .fill(.secondary)
                        }
                        .padding()
                        .upsideDown()
                }
            }
        }
        .upsideDown()
    }
}

struct UpsideDown: ViewModifier {
    func body(content: Content) -> some View {
        content
            .rotationEffect(.radians(Double.pi))
            .scaleEffect(x: -1, y: 1, anchor: .center)
    }
}

extension View {
    func upsideDown() -> some View {
        modifier(UpsideDown())
    }
}
```

And there you have it.

It would be remiss of me not to add a caveat here.
I have absolutely no idea if this is going to break something going forwards, or if there is some kind of weird performance bug at scale. I'm not a SwiftUI engineer, so I may be doing something bad here - I'm sure I'll find out in production.

If there is as issue, I will back it out of my code and do something else. But at first sight it seems to be fine.

Anyway, hope this helps someone.

--MrB

P.S. I have now posted some code to a blog in 2023 - so I can tick off that from this year's goals YAY!
