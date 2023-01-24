---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /test2.html
layout: none
---
<html lang="en">
    <head><meta charset="UTF-8" />
        <title>Text to Speech</title>
    </head>
    <body>
        <input type="text" name="text">
        <a href="#" class="say">Say it!</a>
        <audio src="" class="" hidden></audio>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
        <script>
            $(function(){
                $('a.say').on('click',function(e){
                    e.preventDefault();
                    var text = $('input[name="text"]').val();
                    text = encodeURIComponent(text);
                    console.log(text);
                    var url = "https://translate.google.com/translate_tts?ie=UTF-&&q=" + text + "&tl=en"
                    $('audio').attr('src', url).get(0).play();
                });
            });
        </script>
    </body>
</html>