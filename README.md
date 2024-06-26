# Solid or Pattern (SorP)

AI tells you if your clothe have Patter or not
<br>
<br>click link to run
<br>https://rinaralte-monk.github.io/SorP/

### A look into the index👍

the index compromises of html with flowbite library and javascript with tensorflow.js library

- include tensorflow.js library

```
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
```

<br>

- include flowbite

```
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flowbite/2.3.0/flowbite.min.css" rel="stylesheet" />
    <script src="https://cdnjs.cloudflare.com/ajax/libs/flowbite/2.3.0/flowbite.min.js"></script>
```
<br>

### JS script Breakdown👍


- startStream() starts the webcam then call the predict() func
<br> video <- stores the where you gonna display video
<br> stream <- awaits user allow cam then stores the stream object
<br> vides <- pass the stream
<br> model <- loads the pre-trained model
<br> and at last calls the predict() in an interval of 0.1 sec


```
      startStream()
      let video = document.getElementById('video');

      async function startStream() {
        const stream = await navigator.mediaDevices.getUserMedia({ video: true });
        video.srcObject = stream;

        var model = await tf.loadGraphModel('./model.json');

        predict(model);
        setInterval(() => predict(model), 100);
      };

```

<br>

- predict() function predicts solid or pattern
<br> example <- catch the video, then it is reshaped
<br> use the model we load from before to run the ai
<br> display the output on 'result' paragraph

```
      async function predict(model) {
        let example = tf.browser.fromPixels(video, 3).cast('float32');
        example = example.reshape([1, example.shape[0], example.shape[1], example.shape[2]]);

        let prediction = await model.predict(example);
        let class_scores = await prediction.data();
        let max_score_id = class_scores.indexOf(Math.max(...class_scores));
        let classes = ["Pattern", "Solid"];

        document.getElementById("result").innerHTML = classes[max_score_id].toString();
      };
```

<br>

