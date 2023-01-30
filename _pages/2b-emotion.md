---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /2b-emotion.html
layout: none
---
<html>
    <head>
        <title>Running - Recognizing Facial Expressions in the Browser with Deep Learning using TensorFlow.js</title>
        <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@2.4.0/dist/tf.min.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/face-landmarks-detection@0.0.1/dist/face-landmarks-detection.js"></script>
        <script src="web/fer2013.js"></script>
    </head>
    <body>
        <canvas id="output"></canvas>
        <img id="image" style="
            visibility: hidden;
            width: auto;
            height: auto;
            "/>
        <h1 id="status">Loading...</h1>
        <script>
        function setText( text ) {
            document.getElementById( "status" ).innerText = text;
        }

        async function setImage( url ) {
            return new Promise( res => {
                let image = document.getElementById( "image" );
                image.src = url;
                image.onload = () => {
                    res();
                };
            });
        }

        function shuffleArray( array ) {
            for( let i = array.length - 1; i > 0; i-- ) {
                const j = Math.floor( Math.random() * ( i + 1 ) );
                [ array[ i ], array[ j ] ] = [ array[ j ], array[ i ] ];
            }
        }

        function drawLine( ctx, x1, y1, x2, y2, scale = 1 ) {
            ctx.beginPath();
            ctx.moveTo( x1 * scale, y1 * scale );
            ctx.lineTo( x2 * scale, y2 * scale );
            ctx.stroke();
        }

        function drawTriangle( ctx, x1, y1, x2, y2, x3, y3, scale = 1 ) {
            ctx.beginPath();
            ctx.moveTo( x1 * scale, y1 * scale );
            ctx.lineTo( x2 * scale, y2 * scale );
            ctx.lineTo( x3 * scale, y3 * scale );
            ctx.lineTo( x1 * scale, y1 * scale );
            ctx.stroke();
        }

        function wait( ms ) {
            return new Promise( res => setTimeout( res, ms ) );
        }

        const OUTPUT_SIZE = 500;
        const emotions = [ "angry", "disgust", "fear", "happy", "neutral", "sad", "surprise" ];
        let ferData = [];
        let setIndex = 0;
        let emotionModel = null;

        let output = null;
        let model = null;

        async function predictEmotion( points ) {
            let result = tf.tidy( () => {
                const xs = tf.stack( [ tf.tensor1d( points ) ] );
                return emotionModel.predict( xs );
            });
            let prediction = await result.data();
            result.dispose();
            // Get the index of the maximum value
            let id = prediction.indexOf( Math.max( ...prediction ) );
            return emotions[ id ];
        }

        async function trackFace() {
            // Set to the next training image
            await setImage( ferData[ setIndex ].file );
            const image = document.getElementById( "image" );
            const faces = await model.estimateFaces( {
                input: image,
                returnTensors: false,
                flipHorizontal: false,
            });
            output.drawImage(
                image,
                0, 0, image.width, image.height,
                0, 0, OUTPUT_SIZE, OUTPUT_SIZE
            );

            const scale = OUTPUT_SIZE / image.width;

            let points = null;
            faces.forEach( face => {
                // Draw the bounding box
                const x1 = face.boundingBox.topLeft[ 0 ];
                const y1 = face.boundingBox.topLeft[ 1 ];
                const x2 = face.boundingBox.bottomRight[ 0 ];
                const y2 = face.boundingBox.bottomRight[ 1 ];
                const bWidth = x2 - x1;
                const bHeight = y2 - y1;
                drawLine( output, x1, y1, x2, y1, scale );
                drawLine( output, x2, y1, x2, y2, scale );
                drawLine( output, x1, y2, x2, y2, scale );
                drawLine( output, x1, y1, x1, y2, scale );

                // Add just the nose, cheeks, eyes, eyebrows & mouth
                const features = [
                    "noseTip",
                    "leftCheek",
                    "rightCheek",
                    "leftEyeLower1", "leftEyeUpper1",
                    "rightEyeLower1", "rightEyeUpper1",
                    "leftEyebrowLower", //"leftEyebrowUpper",
                    "rightEyebrowLower", //"rightEyebrowUpper",
                    "lipsLowerInner", //"lipsLowerOuter",
                    "lipsUpperInner", //"lipsUpperOuter",
                ];
                points = [];
                features.forEach( feature => {
                    face.annotations[ feature ].forEach( x => {
                        points.push( ( x[ 0 ] - x1 ) / bWidth );
                        points.push( ( x[ 1 ] - y1 ) / bHeight );
                    });
                });
            });

            if( points ) {
                let emotion = await predictEmotion( points );
                setText( `${setIndex + 1}. Expected: ${ferData[ setIndex ].emotion} vs. ${emotion}` );
            }
            else {
                setText( "No Face" );
            }

            setIndex++;
            await wait( 2000 );
            requestAnimationFrame( trackFace );
        }

        (async () => {
            // Get FER-2013 data from the local web server
            // https://www.kaggle.com/msambare/fer2013
            // The data can be downloaded from Kaggle and placed inside the "web/fer2013" folder
            // Get the lowest number of samples out of all emotion categories
            const minSamples = Math.min( ...Object.keys( fer2013 ).map( em => fer2013[ em ].length ) );
            Object.keys( fer2013 ).forEach( em => {
                shuffleArray( fer2013[ em ] );
                for( let i = 0; i < minSamples; i++ ) {
                    ferData.push({
                        emotion: em,
                        file: fer2013[ em ][ i ]
                    });
                }
            });
            shuffleArray( ferData );

            let canvas = document.getElementById( "output" );
            canvas.width = OUTPUT_SIZE;
            canvas.height = OUTPUT_SIZE;

            output = canvas.getContext( "2d" );
            output.translate( canvas.width, 0 );
            output.scale( -1, 1 ); // Mirror cam
            output.fillStyle = "#fdffb6";
            output.strokeStyle = "#fdffb6";
            output.lineWidth = 2;

            // Load Face Landmarks Detection
            model = await faceLandmarksDetection.load(
                faceLandmarksDetection.SupportedPackages.mediapipeFacemesh
            );
            // Load Emotion Detection
            emotionModel = await tf.loadLayersModel( 'web/model/facemo.json' );

            setText( "Loaded!" );

            trackFace();
        })();
        </script>
    </body>
</html>