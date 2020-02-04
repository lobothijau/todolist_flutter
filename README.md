# Membuat Todo List dengan Flutter

<a href="https://stackoverflow.com/questions/tagged/flutter?sort=votes">
   <img alt="Awesome Flutter" src="https://img.shields.io/badge/Awesome-Flutter-blue.svg?longCache=true&style=flat-square" />
</a>

> Built with [Git Tutor](https://github.com/lesnitsky/git-tutor)

[![lesnitsky.dev](https://lesnitsky.dev/icons/shield.svg?hash=42)](https://lesnitsky.dev?utm_source=todolist_flutter)
[![GitHub stars](https://img.shields.io/github/stars/lesnitsky/todolist_flutter.svg?style=social)](https://github.com/lesnitsky/todolist_flutter)
[![Twitter Follow](https://img.shields.io/twitter/follow/lesnitsky_a.svg?label=Follow%20me&style=social)](https://twitter.com/lesnitsky_a)

Tutorial ini akan menjelaskan proses pembuatan aplikasi todo-list sederhana dengan Flutter. 

## Pendahuluan

Pastikan untuk menyelesaikan [installasi flutter](https://flutter.io/docs/get-started/install)

## Langkah Pertama

Jalankan perintah berikut di terminal

```sh
flutter create todo_list
```

Baris pertama pada kode di bawah akan mengimpor pustaka `material` yang disediakan Flutter. Pustaka ini merupakan implementasi berbagai komponen Material Android. 

📄 lib/main.dart

```diff
+ import 'package:flutter/material.dart';

```

Fungsi di bawah merupakan gerbang utama aplikasi flutter. Saat ini ia hanya berfungsi memanggil fungsi lainnya yaitu `runApp`, tapi kita bisa melakukan banyak hal lainnya (misal, membuat aplikasi menjadi full-screen).

📄 lib/main.dart

```diff
  import 'package:flutter/material.dart';
+
+ void main() => runApp(MyApp());

```

Mari kita lakukan saja (membuat aplikasi full-screen) 😏

📄 lib/main.dart

```diff
  import 'package:flutter/material.dart';
+ import 'package:flutter/services.dart';

- void main() => runApp(MyApp());
+ void main() {
+   SystemChrome.setEnabledSystemUIOverlays([]);
+   runApp(MyApp());
+ }

```

Setiap component di flutter disebut dengan `widget`. Ia bisa saja berjenis `stateless` (hanya tampilan) atau `stateful` (memiliki suatu informasi yang bisa berubah). Komponen utama komponen aplikasi harusnya berjenis stateless, jadi mari kita buat komponen tersebut 

📄 lib/main.dart

```diff
    SystemChrome.setEnabledSystemUIOverlays([]);
    runApp(MyApp());
  }
+
+ class MyApp extends StatelessWidget {}

```

Setiap widget harus meng-override fungsi `build`. Fungsi ini akan mengembalikan kumpulan widget untuk layout aplikasi yang dibuat (misalnya `Container`, `Padding`, `Flex`, dsb.) atau `stateful` widget yang berisi proses bisnis aplikasi 

📄 lib/main.dart

```diff
    runApp(MyApp());
  }

- class MyApp extends StatelessWidget {}
+ class MyApp extends StatelessWidget {
+   @override
+   Widget build(BuildContext context) {
+     return Container();
+   }
+ }

```

Untuk kasus top-level App widget (widget yang paling luar), ia sebagiknya mengembalikan `CupertinoApp` dari paket `'package:flutter/material.dart'`, atau `MaterialApp` dari `'package:flutter/material.dart'`

Kita akan gunakan `material` di tutorial ini

📄 lib/main.dart

```diff
  class MyApp extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
-     return Container();
+     return MaterialApp();
    }
  }

```

Kemudian kita beri sebuah judul

📄 lib/main.dart

```diff
  class MyApp extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
-     return MaterialApp();
+     return MaterialApp(
+       title: 'Todo List',
+     );
    }
  }

```

Lalu kita tambahkan widget `Scaffold` sebagai komponen home

`Scaffold` adalah kelas pembantu dari pustaka `material` yang memberikan layout standar yang sering dipakai oleh aplikasi seperti app bar, floating action button, bottom navigation, dsb.

📄 lib/main.dart

```diff
    Widget build(BuildContext context) {
      return MaterialApp(
        title: 'Todo List',
+       home: Scaffold(
+       ),
      );
    }
  }

```

Sekarang kita akan menambahkan sebuah header yang berfungsi untuk menampilkan judul aplikasi 

📄 lib/main.dart

```diff
      return MaterialApp(
        title: 'Todo List',
        home: Scaffold(
+         appBar: AppBar(title: Text('Todo List')),
        ),
      );
    }

```

Terakhir kita berikan atur agar bagian body aplikasi ini mengarahkan ke widget bernama `TodoList`. Sekarang memang belum ada widget tersebut, akan kita tambahkan nanti

📄 lib/main.dart

```diff
        title: 'Todo List',
        home: Scaffold(
          appBar: AppBar(title: Text('Todo List')),
+         body: TodoList(),
        ),
      );
    }

```

## Me-render list

Stateful widget sederhanaa akan terlihat sebagai berikut

📄 lib/todo_list.dart

```dart
import 'package:flutter/material.dart';

class TodoList extends StatefulWidget {
  @override
  _TodoListState createState() => _TodoListState();
}

class _TodoListState extends State<TodoList> {
  @override
  Widget build(BuildContext context) {
    return Container();
  }
}

```

Widget `TodoList` tersebut akan kita impor

📄 lib/main.dart

```diff
  import 'package:flutter/material.dart';
  import 'package:flutter/services.dart';

+ import 'package:todo_list/todo_list.dart';
+
  void main() {
    SystemChrome.setEnabledSystemUIOverlays([]);
    runApp(MyApp());

```

Sekarang kita buat sebuah kelas model baru bernama `Todo`

📄 lib/todo.dart

```dart
class Todo {
  Todo({this.title, this.isDone = false});

  String title;
  bool isDone;
}

```

kemudian impor ke dalam `TodoList`

📄 lib/todo_list.dart

```diff
  import 'package:flutter/material.dart';
+ import 'package:todo_list/todo.dart';

  class TodoList extends StatefulWidget {
    @override

```

Selanjutnya kita akan meng-extend state `TodoList` dan menyiapkan sebuah list todo

📄 lib/todo_list.dart

```diff
  }

  class _TodoListState extends State<TodoList> {
+   List<Todo> todos = [];
+
    @override
    Widget build(BuildContext context) {
      return Container();

```

Mari gunakan `ListView` untuk menampilkan daftar todo 

📄 lib/todo_list.dart

```diff
  class _TodoListState extends State<TodoList> {
    List<Todo> todos = [];

+   _buildItem() {}
+
    @override
    Widget build(BuildContext context) {
-     return Container();
+     return ListView.builder(
+       itemBuilder: _buildItem,
+       itemCount: todos.length,
+     );
    }
  }

```

Sekarang kita akan implementasi method `_buildItem` yang akan dipanggil setiap kali todo akan di render

Kita akan gunakan `CheckboxListTile` dari pustaka `material` karena widget ini memberikan semua yang kita butuhkan (checkbox menandakan sebuah todo sudah selesai atau belum disertai dengan teks judul)

📄 lib/todo_list.dart

```diff
  class _TodoListState extends State<TodoList> {
    List<Todo> todos = [];

-   _buildItem() {}
+   Widget _buildItem(BuildContext context, int index) {
+     final todo = todos[index];
+
+     return CheckboxListTile(
+     );
+   }

    @override
    Widget build(BuildContext context) {

```

Value indicates if list item should be checked

📄 lib/todo_list.dart

```diff
      final todo = todos[index];

      return CheckboxListTile(
+       value: todo.isDone,
      );
    }


```

Title is a widget which should be rendered in first row. Typically it is a `Text` widget

📄 lib/todo_list.dart

```diff

      return CheckboxListTile(
        value: todo.isDone,
+       title: Text(todo.title),
      );
    }


```

Finally we need to handle taps on every list item

📄 lib/todo_list.dart

```diff
      return CheckboxListTile(
        value: todo.isDone,
        title: Text(todo.title),
+       onChanged: (bool isChecked) {
+         _toggleTodo(todo, isChecked);
+       },
      );
    }


```

`_toggleTodo` implementation is pretty straightforward

📄 lib/todo_list.dart

```diff
  class _TodoListState extends State<TodoList> {
    List<Todo> todos = [];

+   _toggleTodo(Todo todo, bool isChecked) {
+     todo.isDone = isChecked;
+   }
+
    Widget _buildItem(BuildContext context, int index) {
      final todo = todos[index];


```

Let's try to add some mock todos and see if they are rendered correctly

📄 lib/todo_list.dart

```diff
  }

  class _TodoListState extends State<TodoList> {
-   List<Todo> todos = [];
+   List<Todo> todos = [
+     Todo(title: 'Learn Dart'),
+     Todo(title: 'Try Flutter'),
+     Todo(title: 'Be amazed'),
+   ];

    _toggleTodo(Todo todo, bool isChecked) {
      todo.isDone = isChecked;

```

Ok, everything is rendered correctly, but nothing happens when we tap on items, weird..

Let's add a debug print and see if the handler even called

📄 lib/todo_list.dart

```diff
    ];

    _toggleTodo(Todo todo, bool isChecked) {
+     print('${todo.title} ${todo.isDone}');
+
      todo.isDone = isChecked;
    }


```

Console shows items are checked, value `isChecked` is `true`, but checkbox is never rendered

The problem is that we modify our entities, but flutter has no idea this happened, so we need to call `setState`. (Hi there, react fans! 😏)

📄 lib/todo_list.dart

```diff
    ];

    _toggleTodo(Todo todo, bool isChecked) {
-     print('${todo.title} ${todo.isDone}');
-
-     todo.isDone = isChecked;
+     setState(() {
+       todo.isDone = isChecked;
+     });
    }

    Widget _buildItem(BuildContext context, int index) {

```

Now we're good with rendering and updates, time to get rid of mock items and add some ui to add new todos.

Let's add a `FloatingActionButton`

📄 lib/main.dart

```diff
        home: Scaffold(
          appBar: AppBar(title: Text('Todo List')),
          body: TodoList(),
+         floatingActionButton: FloatingActionButton(
+           child: Icon(Icons.add),
+         ),
        ),
      );
    }

```

📄 lib/todo_list.dart

```diff
  }

  class _TodoListState extends State<TodoList> {
-   List<Todo> todos = [
-     Todo(title: 'Learn Dart'),
-     Todo(title: 'Try Flutter'),
-     Todo(title: 'Be amazed'),
-   ];
+   List<Todo> todos = [];

    _toggleTodo(Todo todo, bool isChecked) {
      setState(() {

```

Ok, but what should we do in `onPressed`? We need to access a state of `TodoList` and messing with children state directly from parent _statelsess_ widget doesn't sound like a good idea

📄 lib/main.dart

```diff
          body: TodoList(),
          floatingActionButton: FloatingActionButton(
            child: Icon(Icons.add),
+           onPressed: () {
+             // 😢
+           },
          ),
        ),
      );

```

So let's just move `Scaffold` widget down to `TodoList`

📄 lib/main.dart

```diff
    Widget build(BuildContext context) {
      return MaterialApp(
        title: 'Todo List',
-       home: Scaffold(
-         appBar: AppBar(title: Text('Todo List')),
-         body: TodoList(),
-         floatingActionButton: FloatingActionButton(
-           child: Icon(Icons.add),
-           onPressed: () {
-             // 😢
-           },
-         ),
-       ),
+       home: TodoList(),
      );
    }
  }

```

📄 lib/todo_list.dart

```diff
      );
    }

+   _addTodo() {}
+
    @override
    Widget build(BuildContext context) {
-     return ListView.builder(
-       itemBuilder: _buildItem,
-       itemCount: todos.length,
+     return Scaffold(
+       appBar: AppBar(title: Text('Todo List')),
+       body: ListView.builder(
+         itemBuilder: _buildItem,
+         itemCount: todos.length,
+       ),
+       floatingActionButton: FloatingActionButton(
+         child: Icon(Icons.add),
+         onPressed: _addTodo,
+       ),
      );
    }
  }

```

Now we can show a dialog when user taps on `FloatingActionButton`

📄 lib/todo_list.dart

```diff
      );
    }

-   _addTodo() {}
+   _addTodo() {
+     showDialog(
+       context: context,
+       builder: (BuildContext context) {
+         return AlertDialog(
+           title: Text('New todo'),
+         );
+       },
+     );
+   }

    @override
    Widget build(BuildContext context) {

```

Dialog will contain a text input:

📄 lib/todo_list.dart

```diff
        builder: (BuildContext context) {
          return AlertDialog(
            title: Text('New todo'),
+           content: TextField(),
          );
        },
      );

```

and two action buttons: `Cancel` and `Add`

📄 lib/todo_list.dart

```diff
          return AlertDialog(
            title: Text('New todo'),
            content: TextField(),
+           actions: <Widget>[
+             FlatButton(
+               child: Text('Cancel'),
+             ),
+             FlatButton(
+               child: Text('Add'),
+             ),
+           ],
          );
        },
      );

```

Dialogs are not just overlays, but actually a routes, so to handle `Cancel` action we can just call `.pop` on `Navigator` of current `context`

📄 lib/todo_list.dart

```diff
            actions: <Widget>[
              FlatButton(
                child: Text('Cancel'),
+               onPressed: () {
+                 Navigator.of(context).pop();
+               },
              ),
              FlatButton(
                child: Text('Add'),

```

Now we need to access the value of a `TextField` to create a `Todo`
To do this we need to create a `TextEditingController`

📄 lib/todo_list.dart

```diff
  class _TodoListState extends State<TodoList> {
    List<Todo> todos = [];

+   TextEditingController controller = new TextEditingController();
+
    _toggleTodo(Todo todo, bool isChecked) {
      setState(() {
        todo.isDone = isChecked;

```

and supply it to the `TextField`

📄 lib/todo_list.dart

```diff
        builder: (BuildContext context) {
          return AlertDialog(
            title: Text('New todo'),
-           content: TextField(),
+           content: TextField(controller: controller),
            actions: <Widget>[
              FlatButton(
                child: Text('Cancel'),

```

now in `onPressed` of `Add` action we can log the value of a `TextField` and clear it

📄 lib/todo_list.dart

```diff
              ),
              FlatButton(
                child: Text('Add'),
+               onPressed: () {
+                 print(controller.value.text);
+                 controller.clear();
+               },
              ),
            ],
          );

```

Finally let's actually create new todo and add it to the list of existing todos (don't forget to wrap the code with `setState`)

📄 lib/todo_list.dart

```diff
              FlatButton(
                child: Text('Add'),
                onPressed: () {
-                 print(controller.value.text);
-                 controller.clear();
+                 setState(() {
+                   final todo = new Todo(title: controller.value.text);
+
+                   todos.add(todo);
+                   controller.clear();
+
+                   Navigator.of(context).pop();
+                 });
                },
              ),
            ],

```

Tiny UX improvement: make keyboard pop automatically by passing `autofocus: true` to a `TextField`

📄 lib/todo_list.dart

```diff
        builder: (BuildContext context) {
          return AlertDialog(
            title: Text('New todo'),
-           content: TextField(controller: controller),
+           content: TextField(
+             controller: controller,
+             autofocus: true,
+           ),
            actions: <Widget>[
              FlatButton(
                child: Text('Cancel'),

```

## Refactoring

`TodoList`is working, but `todo_list.dart` is kinda messy and hard to read. The most complex method is `_addTodo`, so let's start with rewriting it. It seems like we can move the `AlertDialog` to a separate widget, but we can't do this right now, as we rely on `setState` from parent widget. Instead we can pass a freshly created todo to a `Navigator.pop`

📄 lib/todo_list.dart

```diff
    }

    _addTodo() {
-     showDialog(
+     showDialog<Todo>(
        context: context,
        builder: (BuildContext context) {
          return AlertDialog(
              FlatButton(
                child: Text('Add'),
                onPressed: () {
-                 setState(() {
-                   final todo = new Todo(title: controller.value.text);
+                 final todo = new Todo(title: controller.value.text);
+                 controller.clear();

-                   todos.add(todo);
-                   controller.clear();
-
-                   Navigator.of(context).pop();
-                 });
+                 Navigator.of(context).pop(todo);
                },
              ),
            ],

```

In order to be able to receive the `Todo` in `_addTodo` method we need to make it `async` and `await` `showDialog` function result (which will be `null` in case it was dismissed and instance of `Todo` otherwise)

📄 lib/todo_list.dart

```diff
      );
    }

-   _addTodo() {
-     showDialog<Todo>(
+   _addTodo() async {
+     final todo = await showDialog<Todo>(
        context: context,
        builder: (BuildContext context) {
          return AlertDialog(

```

And move back the logic with state update

📄 lib/todo_list.dart

```diff
          );
        },
      );
+
+     if (todo != null) {
+       setState(() {
+         todos.add(todo);
+       });
+     }
    }

    @override

```

Now we don't have any dependencies on a parent widget, so we can extract `AlertDialog` to a separate widget

📄 lib/new_todo_dialog.dart

```dart
import 'package:flutter/material.dart';

import 'package:todo_list/todo.dart';

class NewTodoDialog extends StatelessWidget {
  final controller = new TextEditingController();

  @override
  Widget build(BuildContext context) {
    return AlertDialog(
      title: Text('New todo'),
      content: TextField(
        controller: controller,
        autofocus: true,
      ),
      actions: <Widget>[
        FlatButton(
          child: Text('Cancel'),
          onPressed: () {
            Navigator.of(context).pop();
          },
        ),
        FlatButton(
          child: Text('Add'),
          onPressed: () {
            final todo = new Todo(title: controller.value.text);
            controller.clear();

            Navigator.of(context).pop(todo);
          },
        ),
      ],
    );
  }
}

```

and use it inside `TodoList`

📄 lib/todo_list.dart

```diff
  import 'package:flutter/material.dart';
  import 'package:todo_list/todo.dart';

+ import 'package:todo_list/new_todo_dialog.dart';
+
  class TodoList extends StatefulWidget {
    @override
    _TodoListState createState() => _TodoListState();
  class _TodoListState extends State<TodoList> {
    List<Todo> todos = [];

-   TextEditingController controller = new TextEditingController();
-
    _toggleTodo(Todo todo, bool isChecked) {
      setState(() {
        todo.isDone = isChecked;
      final todo = await showDialog<Todo>(
        context: context,
        builder: (BuildContext context) {
-         return AlertDialog(
-           title: Text('New todo'),
-           content: TextField(
-             controller: controller,
-             autofocus: true,
-           ),
-           actions: <Widget>[
-             FlatButton(
-               child: Text('Cancel'),
-               onPressed: () {
-                 Navigator.of(context).pop();
-               },
-             ),
-             FlatButton(
-               child: Text('Add'),
-               onPressed: () {
-                 final todo = new Todo(title: controller.value.text);
-                 controller.clear();
-
-                 Navigator.of(context).pop(todo);
-               },
-             ),
-           ],
-         );
+         return NewTodoDialog();
        },
      );


```

Next step – extract todo list component

List istself could also be treated as stateless widget, state related logic could be handled by parent

So let's first rename `TodoList` to `TodoListScreen`

📄 lib/todo_list.dart

```diff

  import 'package:todo_list/new_todo_dialog.dart';

- class TodoList extends StatefulWidget {
+ class TodoListScreen extends StatefulWidget {
    @override
-   _TodoListState createState() => _TodoListState();
+   _TodoListScreenState createState() => _TodoListScreenState();
  }

- class _TodoListState extends State<TodoList> {
+ class _TodoListScreenState extends State<TodoListScreen> {
    List<Todo> todos = [];

    _toggleTodo(Todo todo, bool isChecked) {

```

rename file

📄 lib/todo_list_screen.dart => lib/todo_list.dart

and fix import

📄 lib/main.dart

```diff
  import 'package:flutter/material.dart';
  import 'package:flutter/services.dart';

- import 'package:todo_list/todo_list.dart';
+ import 'package:todo_list/todo_list_screen.dart';

  void main() {
    SystemChrome.setEnabledSystemUIOverlays([]);
    Widget build(BuildContext context) {
      return MaterialApp(
        title: 'Todo List',
-       home: TodoList(),
+       home: TodoListScreen(),
      );
    }
  }

```

Let's move list related logic to a separate stateless widget

📄 lib/todo_list.dart

```dart
import 'package:flutter/material.dart';

class TodoList extends StatelessWidget {
  _toggleTodo(Todo todo, bool isChecked) {
    setState(() {
      todo.isDone = isChecked;
    });
  }

  Widget _buildItem(BuildContext context, int index) {
    final todo = todos[index];

    return CheckboxListTile(
      value: todo.isDone,
      title: Text(todo.title),
      onChanged: (bool isChecked) {
        _toggleTodo(todo, isChecked);
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemBuilder: _buildItem,
      itemCount: todos.length,
    );
  }
}

```

and remove this logic from `TodoListScreen`

📄 lib/todo_list_screen.dart

```diff
  import 'package:todo_list/todo.dart';

  import 'package:todo_list/new_todo_dialog.dart';
+ import 'package:todo_list/todo_list.dart';

  class TodoListScreen extends StatefulWidget {
    @override
  class _TodoListScreenState extends State<TodoListScreen> {
    List<Todo> todos = [];

-   _toggleTodo(Todo todo, bool isChecked) {
-     setState(() {
-       todo.isDone = isChecked;
-     });
-   }
-
-   Widget _buildItem(BuildContext context, int index) {
-     final todo = todos[index];
-
-     return CheckboxListTile(
-       value: todo.isDone,
-       title: Text(todo.title),
-       onChanged: (bool isChecked) {
-         _toggleTodo(todo, isChecked);
-       },
-     );
-   }
-
    _addTodo() async {
      final todo = await showDialog<Todo>(
        context: context,
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(title: Text('Todo List')),
-       body: ListView.builder(
-         itemBuilder: _buildItem,
-         itemCount: todos.length,
-       ),
+       body: TodoList(),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.add),
          onPressed: _addTodo,

```

Now let's review our `TodoList` widget

It is missing `Todo` class import

📄 lib/todo_list.dart

```diff
  import 'package:flutter/material.dart';

+ import 'package:todo_list/todo.dart';
+
  class TodoList extends StatelessWidget {
    _toggleTodo(Todo todo, bool isChecked) {
      setState(() {

```

It also doesn't have `todos`, so let's pass them from parent widget

📄 lib/todo_list.dart

```diff
  import 'package:todo_list/todo.dart';

  class TodoList extends StatelessWidget {
+   TodoList({@required this.todos});
+
+   final List<Todo> todos;
+
    _toggleTodo(Todo todo, bool isChecked) {
      setState(() {
        todo.isDone = isChecked;

```

📄 lib/todo_list_screen.dart

```diff
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(title: Text('Todo List')),
-       body: TodoList(),
+       body: TodoList(
+         todos: todos,
+       ),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.add),
          onPressed: _addTodo,

```

`_toggleTodo` method relies on `setState`, so let's move it back to parent

📄 lib/todo_list.dart

```diff

    final List<Todo> todos;

-   _toggleTodo(Todo todo, bool isChecked) {
-     setState(() {
-       todo.isDone = isChecked;
-     });
-   }
-
    Widget _buildItem(BuildContext context, int index) {
      final todo = todos[index];


```

📄 lib/todo_list_screen.dart

```diff
  class _TodoListScreenState extends State<TodoListScreen> {
    List<Todo> todos = [];

+   _toggleTodo(Todo todo, bool isChecked) {
+     setState(() {
+       todo.isDone = isChecked;
+     });
+   }
+
    _addTodo() async {
      final todo = await showDialog<Todo>(
        context: context,

```

and pass it down to `TodoList` as a property

📄 lib/todo_list.dart

```diff

  import 'package:todo_list/todo.dart';

+ typedef ToggleTodoCallback = void Function(Todo, bool);
+
  class TodoList extends StatelessWidget {
-   TodoList({@required this.todos});
+   TodoList({@required this.todos, this.onTodoToggle});

    final List<Todo> todos;
+   final ToggleTodoCallback onTodoToggle;

    Widget _buildItem(BuildContext context, int index) {
      final todo = todos[index];
        value: todo.isDone,
        title: Text(todo.title),
        onChanged: (bool isChecked) {
-         _toggleTodo(todo, isChecked);
+         onTodoToggle(todo, isChecked);
        },
      );
    }

```

📄 lib/todo_list_screen.dart

```diff
        appBar: AppBar(title: Text('Todo List')),
        body: TodoList(
          todos: todos,
+         onTodoToggle: _toggleTodo,
        ),
        floatingActionButton: FloatingActionButton(
          child: Icon(Icons.add),

```

## Conclusion

Yay! We have working and kinda well-structured Todo List application written in Flutter 🎉

But there is still a lot of work to do:

![App-Screenshot-3.png](https://s3.eu-west-2.amazonaws.com/git-tutor-assets/git-tutor-todolist-3.png)

See you in next tutorials! 👋

> Built with [Git Tutor](https://github.com/lesnitsky/git-tutor)


[![lesnitsky.dev](https://lesnitsky.dev/icons/shield.svg?hash=42)](https://lesnitsky.dev?utm_source=todolist_flutter)
[![GitHub stars](https://img.shields.io/github/stars/lesnitsky/todolist_flutter.svg?style=social)](https://github.com/lesnitsky/todolist_flutter)
[![Twitter Follow](https://img.shields.io/twitter/follow/lesnitsky_a.svg?label=Follow%20me&style=social)](https://twitter.com/lesnitsky_a)
