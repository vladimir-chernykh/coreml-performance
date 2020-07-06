# CoreML Performance

This repo allows to test the **performance** of Machine Learning models in **CoreML** *\*.mlmodel* format.

The code was initially designed to work with all the models in [**CoreML Model Zoo**](https://github.com/vladimir-chernykh/coreml-model-zoo). But with a bit of tweaking it could work for any CoreML model.

# Quick Start

1. **Clone** the project
   ```bash
   git clone https://github.com/vladimir-chernykh/coreml-performance.git
   ```
2. Open **XCode**. The code was tested in XCode 11.5.
3. Go to *File -> Open* and choose *CoreMLPerformance.xcodeproj* in the repo just cloned.
4. **Build** and **Run** the project using either *simulator* or *real device*.
5. The results of the performance testing will be **printed to console**:
   * Latency
   * RPS (= 1 / Latency)

![results in XCode](./misc/results.jpg)

# Setting

The performnace is measured in the following setting:
* One image is used. It is **loaded and preprocessed in advance**.
* The time is measured `numIter` times **only** for the **prediction** operation
   ```swift
   let startTime = CACurrentMediaTime()
   for _ in 0..<numIter {
       _ = try! model.prediction(input: input)
   }
   let endTime = CACurrentMediaTime()
   ```
   Full code is in the [ViewController.swift](./CoreMLPerformance/ViewController.swift)
* When the real measurement is taken, first **50 iterations** are done as a **warmup**. All the **real** time metrics are measured and averaged for the following **500 iterations**.
* Each model is tested with all possible computing devices provided in Apple products:
   * **ANE** which stands for **A**pple **N**eural **E**ngine and is the Apple's custom Neural Processing Unit. It was specifically designed to speed up on-device neural nets computations. This is currently supported only by mobile processors starting from A11 Bionic (but recently Apple announced ANE support in new Silicon processors lineup)
   * **GPU**
   * **CPU**
