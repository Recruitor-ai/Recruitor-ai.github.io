---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /chatbot.html
layout: none
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Test Bot</title>
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <style>
        .bg-half{
            min-height:50vh;
        }
        .mh-40{
            min-height: 40vh;
        }

    </style>
    <link rel="stylesheet" href="./css/bootstrap.min.css">
</head>
<body>
    <div class="min-vw-100 bg-half bg-info"></div>
    <div class="vw-100 bg-half bg-danger"></div>
    <div class="vw-100 bg-half bg-warning"></div>
    <button class="btn bg-info offset-10 col-lg-1 fixed-bottom text-light ">Talk To Some one</button>
    <div class="">
        <div class="fixed-bottom offset-lg-7 col-xl-3 col-lg-4 offset-md-4 col-md-7 bg-dark mh-40 card rounded-0">

                <h5 class=" text-monospace card-header bg-danger">Chat With BOT</h5>
            <div class="chat-box h-auto text-center card-body ">
            </div>
            <label class="input-group  ">

                <textarea class="rounded-left border-0 col-10 card-footer text-warning" placeholder="Send Message..." ></textarea>
                <input class="rounded-right border-0 bg-success " type="submit" value=">>>" onclick="env.reply()">
            </label>
        </div>
    </div>
    <script >
        let env = {
            res:["js/templates/user","js/templates/incoming"],
            init:()=>{
                let loader  = async ()=>{
                    let htmlArray = [];
                    for (let a of env.res ) {
                        htmlArray.push(await (await fetch(a,{method:"GET",credentials:"same-origin"})).text());
                    }
                    return htmlArray;
                };
                let initRunner = async () =>{
                    env.msgUI = await loader();
                    return true;
                };
                initRunner().then(e=>{console.log(e)});
            },
            reply:() => {
                const msg = document.querySelector("textarea").value;
                env.display(msg,0);
            },
            incoming:(msg = "hello, how may I help?")=>{
                env.display(msg);
            },
            send:()=>{},
            display:(msg,type=1)=>{
                let chatBox = document.querySelector(".chat-box");
                chatBox.innerHTML+=env.msgUI[type];
                let last = chatBox.querySelectorAll(".msg").length;
                chatBox.querySelectorAll(".msg")[last - 1].textContent = msg;
                document.querySelector("textarea").value = "";
            }
        };

        env.init();
    </script>
</body>
</html>