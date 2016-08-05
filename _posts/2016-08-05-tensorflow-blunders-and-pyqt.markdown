---
published: true
title: TensorFlow blunders and PyQt
layout: post
---
I have a little blog idea that sits in my notes for half a month now and I decided to blog about it. It's not like I've been super busy or anything; I just got lazy, lazy to organize what I'm trying to talk about. So a while ago I couldn't get a reasonable accuracy from a TensorFlow neural network model, and I finally figured out it's the evaluation that had gone wrong (after a very long time of aimlessly messing about).

This might be very natural: if an array of ops are fed into a `session.run(...)` call, they will be evaluated in one single run of graph computation; but if N ops are called separately, the graph is going to be evaluated N times. So, somehow I did this in my code:

```
images, labels = inputs(...)
inference_op = inference(images)
...
predict = sess.run(inference_op)
[some processing]
ground_truth = sess.run(labels)
[some processing]
```

The catch here is `label` comes from the input pipeline (there's a very nice little [GIF](https://www.tensorflow.org/versions/r0.10/how_tos/reading_data/index.html#creating-threads-to-prefetch-using-queuerunner-objects) illustration of how the input feed works), and `inputs()` generates a new set of images with labels each time the graph is evaluated. The two separate calls produce mismatched predictions and ground truth values and I spent like a week or so wondering why my model is worse than pure random guess. The fix is simple:

```
predict, ground_truth = sess.run([inference_op, labels])
```

I managed to write a little TensorFlow data [parser](https://github.com/kwanz/tf-mat-parser) for Matlab MAT some time ago because I had a bunch of MAT files I might use and I was thinking to do it in a TensorFlow-graph fashion. Well, it's much easier to use `scipy.io` to load MAT and convert it to tensor, but if I want to use the input pipeline and have the queue runners take care of batch file I/O - which is the whole point of the TensorFlow graph paradigm - I will need an op, and it's C++ I will have to deal with. Long story short, I did get the op working, but I can't use it because my data is `uint32` format and TensorFlow is missing a few lines of code to support the type ([#1894](https://github.com/tensorflow/tensorflow/issues/1894)). I can see in the source code it's a TODO thing to fill in the types, but I think it's a weird thing to miss out or temporarily not have.

One last thing: I'm writing a GUI using PyQt recently and I really like this combination. Qt is a decent GUI library and Python is quick to write and capable of doing all kinds of elegant one-liners. The only drawback is it's having both the Python 2/3 and Qt 4/5 situations at the same time and a ton of online references may not work. I guess it's kind of a good thing for a language to just move on and evolve (though I'm not sure how much of the new stuff the programming language people actually appreciate), but I work with Python 3 + Qt 4 because some partner decided to randomly pick a set up and I find it super awkward and annoying.
