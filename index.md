<!DOCTYPE html>

<html lang="en" xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="utf-8" />
    <title></title>
</head>
<style>
    p:hover
    {background-color:Highlight;}

    a
    {text-decoration:underline;
      color: blue;
      cursor: pointer;
    }
    body {
        background-color: dimgrey;
    }

    #score {
        width: 150px;
        height: 600px;
        background-color: linen;
        border: 2px solid black;
        float: right;
    }

    #bord {
        width: 350px;
        height: 350px;
        background-color: linen;
        border: solid black 2px;
    }

    div.lamp {
        border-radius: 50%;
        width: 50px;
        height: 50px;
        border: lightblue 4px solid;
        background-color: #3e3e3e;
        float: left;
        margin: 20px;
    }

    div.lightOn {
        background-color: yellow;
    }

    div.first {
        clear: left;
    }
</style>
<body>
   
    <div id="score">
        <div>
           
        </div>
        <div id="todo">loading...</div>

    </div>
    <div id="bord">
        Name:</br>
        <input type="text" onkeydown="if(event.keyCode === 13) add(this);" />
        <div id="info"></div>
        <div class="lamp first" onclick="turnOff(this)"></div>
        <div class="lamp" onclick="turnOff(this)"></div>
        <div class="lamp" onclick="turnOff(this)"></div>


        <div class="lamp first" onclick="turnOff(this)"></div>
        <div class="lamp" onclick="turnOff(this)"></div>
        <div class="lamp" onclick="turnOff(this)"></div>


        <div class="lamp first" onclick="turnOff(this)"></div>
        <div class="lamp" onclick="turnOff(this)"></div>
        <div class="lamp" onclick="turnOff(this)"></div>
    </div>

    <script src="https://www.gstatic.com/firebasejs/4.8.1/firebase.js"></script>
    <script src="https://www.gstatic.com/firebasejs/4.8.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/4.8.1/firebase-firestore.js"></script>
    <script>

        var startTime = new Date().getTime();
        turnOnRandom();
        function turnOnRandom() {
            var elementsList = document.getElementsByClassName("lamp");
            var randomIndex = Math.floor(Math.random() * elementsList.length);
            var selectedElement = elementsList[randomIndex];
            var selectedElementClassList = selectedElement.classList;
            selectedElementClassList.add("lightOn");
        }
        function turnOff(selectedElement) {
            var selectedElementClassList = selectedElement.classList;
            if (selectedElementClassList.contains("lightOn")) {
                var finishTime = new Date().getTime();
                var spentMilliseconds = Math.floor(finishTime - startTime);
                var spentSeconds = spentMilliseconds / 1000;
                document.getElementById('info').innerHTML = 'Du brukte ' + spentSeconds + ' sekunder.'
                startTime = new Date().getTime();
                selectedElementClassList.remove("lightOn");
                turnOnRandom();
            }
        }







        // Initialize Firebase
        var config = {
            apiKey: "AIzaSyCAIBOvkU-zwolSCYHslEOkJodGDz5vu70",
            authDomain: "min-jatakk.firebaseapp.com",
            databaseURL: "https://min-jatakk.firebaseio.com",
            projectId: "min-jatakk",
            storageBucket: "",
            messagingSenderId: "924608462819"
        };
        firebase.initializeApp(config);
        var db = firebase.firestore();

        var todoCollection = db.collection('todoItems');
        //  db.collection('todoItems').get().then(
        todoCollection.onSnapshot(
            function (collectionSnapshot) {


                let html = '<ul>';
                collectionSnapshot.forEach(
                    function (todoItemsSnapshot) {
                        let todoItem = todoItemsSnapshot.data();
                        html += '<p>' + todoItem.todoText + ' (' + todoItem.todoDate + ')  </p>';
                    });
                html += '</ul>';
                document.getElementById('todo').innerHTML = html;

            });
        function add(inputTag) {
            var d = new Date(); 
          


            let todoItem = {
                todoText: inputTag.value,
                todoDate: d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds() 
                
            };
            todoCollection.add(todoItem);
        }

       
    </script>
</body>
</html>
