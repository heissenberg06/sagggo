# sagggo
asd

import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {

    return MaterialApp(
        title: 'Book App',
        debugShowCheckedModeBanner: false,
        theme: ThemeData(
          primarySwatch: Colors.brown,
        ),
        home: CategoryScreen(),
        routes: {
          'category-books': (ctx) => BookCategoryScreen(),
        });
  }
}

class CategoryScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: const Icon(Icons.book_outlined),
        title: const Text('BOOKS'),
      ),
      body: GridView(
        gridDelegate:  const SliverGridDelegateWithMaxCrossAxisExtent(
          maxCrossAxisExtent:
          200,
          childAspectRatio:
          3 / 2,
          crossAxisSpacing: 20,
          mainAxisSpacing: 20,
        ),
        children: DUMMY_CATEGORIES
            .map((bookData) =>
            CategoryItem(bookData.id, bookData.title, bookData.color))
            .toList(),
      ),
    );
  }
}

class CategoryItem extends StatefulWidget {
  final String id;
  final String title;
  final Color color;
  CategoryItem(this.id, this.title, this.color);

  @override
  State<CategoryItem> createState() => _CategoryItemState();
}

class _CategoryItemState extends State<CategoryItem> {
  void selectCategory(BuildContext ctx) {
    Navigator.of(ctx).pushNamed(
      'category-books',
      arguments: {
        'id': widget.id,
        'title': widget.title,
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return InkWell(
      onTap: () => selectCategory(context),
      borderRadius: BorderRadius.circular(10),
      child: Container(
        alignment: Alignment.center,
        child: Text(
          widget.title,
          style: TextStyle(
            fontSize: 22,
            color: Colors.white,
          ),
        ),
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [
              widget.color.withOpacity(0.7),
              widget.color,
            ],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
          borderRadius: BorderRadius.circular(15),
        ),
      ),
    );
  }
}

class BookCategoryScreen extends StatefulWidget {

  @override
  State<BookCategoryScreen> createState() => _BookCategoryScreenState();
}

class _BookCategoryScreenState extends State<BookCategoryScreen> {

  final titleController = TextEditingController();

  void _addBook(String title, String category){
    Book newBook = Book(id: 'b${DUMMY_BOOKS.length+1}', title: title, categories: [category]);
    setState(() {
      DUMMY_BOOKS.add(newBook);
    });
    Navigator.of(context).pop();
  }

  void _startAddNewBooks(BuildContext ctx, String categoryTitle){
    showModalBottomSheet(
      context: ctx,
      builder: (ctx){
        return GestureDetector(
          behavior: HitTestBehavior.opaque,
          onTap: (){},
          child: Card(
            elevation: 5,
            child: Column(
              children: [
                TextField(
                  decoration: const InputDecoration(labelText: 'Title'),
                  controller: titleController,
                ),
                TextButton(
                  child: const Text('Add Book'),
                  onPressed: () {
                    _addBook(
                        titleController.text,
                        categoryTitle,
                    );
                    titleController.clear();
                  },
                ),
              ],
            ),
          ),
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    final routeArgs =
    ModalRoute.of(context)!.settings.arguments as Map<String, String>;
    final String categoryTitle = routeArgs['title']!;
    final String categoryId = routeArgs['id']!;

    final categoryBooks = DUMMY_BOOKS.where( (book){
      return book.categories.contains(categoryId);
    }).toList();

    return Scaffold(
      appBar: AppBar(
        title: Text(categoryTitle),
      ),
      body: Center(
        child: ListView.builder(
          itemBuilder: (ctx,index){
            return Card(
              child: ListTile(
                title: Text(
                  categoryBooks[index].title,
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    color: Colors.brown,
                  ),
                ),
              ),
            );
          },
          itemCount: categoryBooks.length,
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        backgroundColor: Colors.brown[100],
        items:  [
          BottomNavigationBarItem(
              icon: IconButton(onPressed: (){
                Navigator.of(context).pop();
                },
                  icon: Icon(Icons.home)),
            label: 'Home',
          ),
          BottomNavigationBarItem(
              icon: IconButton(
                  onPressed: (){
                    _startAddNewBooks(context, categoryId);
                    },
                  icon: Icon(Icons.add)),
            label: 'Add',
          ),
        ],
      ),
    );
  }
}

class Book{

  final String id;
  final String title;
  List<String> categories;

  Book({
    required this.id,
    required this.title,
    required this.categories
});

}

class Category {
  final String id;
  final String title;
  final Color color;
  const Category(
      {required this.id, required this.title, this.color = Colors.orange});
}



const DUMMY_CATEGORIES = [
  Category(
    id: 'c1',
    title: 'Literature',
    color: Colors.purple,
  ),
  Category(
    id: 'c2',
    title: 'Self-Help',
    color: Colors.red,
  ),
  Category(
    id: 'c3',
    title: 'Horror',
    color: Colors.orange,
  ),
  Category(
    id: 'c4',
    title: 'History',
    color: Colors.amber,
  ),
  Category(
    id: 'c5',
    title: 'Mysteries',
    color: Colors.blue,
  ),
  Category(
    id: 'c6',
    title: 'Romance',
    color: Colors.green,
  ),
  Category(
    id: 'c7',
    title: 'Westerns',
    color: Colors.lightBlue,
  ),
  Category(
    id: 'c8',
    title: 'Comics',
    color: Colors.lightGreen,
  ),
  Category(
    id: 'c9',
    title: 'Health anf Fitness',
    color: Colors.pink,
  ),
  Category(
    id: 'c10',
    title: 'Hobbies and Crafts',
    color: Colors.teal,
  ),
  Category(
    id: 'c11',
    title: 'Religion',
    color: Colors.purpleAccent,
  ),
  Category(
    id: 'c12',
    title: 'Science Fiction (Sci-Fi)',
    color: Colors.cyanAccent,
  ),
  Category(
    id: 'c13',
    title: 'Short Stories',
    color: Colors.pink,
  ),
  Category(
    id: 'c14',
    title: 'Suspense and Thrillers',
    color: Colors.amber,
  ),
  Category(
    id: 'c15',
    title: 'Home and Garden',
    color: Colors.black12,
  ),
  Category(
    id: 'c16',
    title: 'Medical',
    color: Colors.teal,
  ),
  Category(
    id: 'c17',
    title: 'Parenting',
    color: Colors.pink,
  ),

];

final DUMMY_BOOKS =  [
  Book(
    id: 'b1',
    categories: [
      'c1',
      'c3',
    ],
    title: 'Treasure Island -  Robert Louis Stevenson',
  ),

  Book(
    id: 'b2',
    categories: [
      'c1',
    ],
    title: 'Little Women and Other Novels - Louisa May Alcott',
  ),

  Book(
    id: 'b3',
    categories: [
      'c1',
    ],
    title: 'Frankenstein - Mary Shelley',
  ),

  Book(
    id: 'b4',
    categories: [
      'c1',
    ],
    title: 'To Kill a Mockingbird - Harper Lee',
  ),

  Book(
    id: 'b5',
    categories: [
      'c1',
    ],
    title: 'Pride and Prejudice - Jane Austen',
  ),

  Book(
    id: 'b6',
    categories: [
      'c1',
    ],
    title: 'Pride and Prejudice - Jane Austen',
  ),

  Book(
    id: 'b7',
    categories: [
      'c1',
    ],
    title: 'Pride and Prejudice - Jane Austen',
  ),
  Book(
    id: 'b8',
    categories: [
      'c1',
    ],
    title: 'Pride and Prejudice - Jane Austen',
  ),

  Book(
    id: 'b9',
    categories: [
      'c1',
    ],
    title: 'Pride and Prejudice - Jane Austen',
  ),

  Book(
    id: 'b10',
    categories: [
      'c2',
    ],
    title: 'The Alchemist by Paulo Coelho',
  ),
  Book(
    id: 'b11',
    categories: [
      'c2',
    ],
    title: 'Atomic Habits by James Clear',
  ),
  Book(
    id: 'b12',
    categories: [
      'c2',
    ],
    title: 'Thinking Fast And Slow by Daniel Kahneman',
  ),
  Book(
    id: 'b13',
    categories: [
      'c2',
    ],
    title: 'The Four Agreements by Don Miguel Ruiz',
  ),
  Book(
    id: 'b14',
    categories: [
      'c2',
    ],
    title: 'The 7 Habits Of Highly Effective People by Stephen R. Covey',
  ),
  Book(
    id: 'b15',
    categories: [
      'c2',
    ],
    title: 'Best Self by Mike Bayer',
  ),

  Book(
    id: 'b16',
    categories: [
      'c2',
    ],
    title: 'The Subtle Art of Not Giving a F*ck by Mark Manson Best Self Help Books for Women',
  ),

  Book(
    id: 'b17',
    categories: [
      'c2',
    ],
    title: 'Girl, Wash Your Face by Rachel Hollis',
  ),

  Book(
    id: 'b18',
    categories: [
      'c2',
    ],
    title: '12 Rules For Life by Jordan Peterson',
  ),
  Book(
    id: 'b19',
    categories: [
      'c2',
    ],
    title: 'Big Magic by Elizabeth Gilbert',
  ),

];


/*******************************/***********

class CategoryScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: const Icon(Icons.book_outlined),
        title: const Text('BOOKS'),
      ),
      body: GridView(
        gridDelegate:  const SliverGridDelegateWithMaxCrossAxisExtent(
          maxCrossAxisExtent:
          200,
          childAspectRatio:
          3 / 2,
          crossAxisSpacing: 20,
          mainAxisSpacing: 20,
        ),
        children: DUMMY_CATEGORIES
            .map((bookData) =>
            CategoryItem(bookData.id, bookData.title, bookData.color))
            .toList(),
      ),
    );
  }
}


/**************************/******************************

class CategoryItem extends StatefulWidget {
  final String id;
  final String title;
  final Color color;
  CategoryItem(this.id, this.title, this.color);

  @override
  State<CategoryItem> createState() => _CategoryItemState();
}

class _CategoryItemState extends State<CategoryItem> {
  void selectCategory(BuildContext ctx) {
    Navigator.of(ctx).pushNamed(
      'category-books',
      arguments: {
        'id': widget.id,
        'title': widget.title,
      },
    );
  }


/*****************/******************
  Widget build(BuildContext context) {
    return InkWell(
      onTap: () => selectCategory(context),
      borderRadius: BorderRadius.circular(10),
      child: Container(
        alignment: Alignment.center,
        child: Text(
          widget.title,
          style: TextStyle(
            fontSize: 22,
            color: Colors.white,
          ),
        ),
        decoration: BoxDecoration(
          gradient: LinearGradient(
            colors: [
              widget.color.withOpacity(0.7),
              widget.color,
            ],
            begin: Alignment.topLeft,
            end: Alignment.bottomRight,
          ),
          borderRadius: BorderRadius.circular(15),
        ),
      ),
    );
  }
}

class BookCategoryScreen extends StatefulWidget {

  @override
  State<BookCategoryScreen> createState() => _BookCategoryScreenState();
}

class _BookCategoryScreenState extends State<BookCategoryScreen> {

  final titleController = TextEditingController();

  void _addBook(String title, String category){
    Book newBook = Book(id: 'b${DUMMY_BOOKS.length+1}', title: title, categories: [category]);
    setState(() {
      DUMMY_BOOKS.add(newBook);
    });
    Navigator.of(context).pop();
  }

  void _startAddNewBooks(BuildContext ctx, String categoryTitle){
    showModalBottomSheet(
      context: ctx,
      builder: (ctx){
        return GestureDetector(
          behavior: HitTestBehavior.opaque,
          onTap: (){},
          child: Card(
            elevation: 5,
            child: Column(
              children: [
                TextField(
                  decoration: const InputDecoration(labelText: 'Title'),
                  controller: titleController,
                ),
                TextButton(
                  child: const Text('Add Book'),
                  onPressed: () {
                    _addBook(
                        titleController.text,
                        categoryTitle,
                    );
                    titleController.clear();
                  },
                ),
              ],
            ),
          ),
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    final routeArgs =
    ModalRoute.of(context)!.settings.arguments as Map<String, String>;
    final String categoryTitle = routeArgs['title']!;
    final String categoryId = routeArgs['id']!;

    final categoryBooks = DUMMY_BOOKS.where( (book){
      return book.categories.contains(categoryId);
    }).toList();

    return Scaffold(
      appBar: AppBar(
        title: Text(categoryTitle),
      ),
      body: Center(
        child: ListView.builder(
          itemBuilder: (ctx,index){
            return Card(
              child: ListTile(
                title: Text(
                  categoryBooks[index].title,
                  style: TextStyle(
                    fontWeight: FontWeight.bold,
                    color: Colors.brown,
                  ),
                ),
              ),
            );
          },
          itemCount: categoryBooks.length,
        ),
      ),
      bottomNavigationBar: BottomNavigationBar(
        backgroundColor: Colors.brown[100],
        items:  [
          BottomNavigationBarItem(
              icon: IconButton(onPressed: (){
                Navigator.of(context).pop();
                },
                  icon: Icon(Icons.home)),
            label: 'Home',
          ),
          BottomNavigationBarItem(
              icon: IconButton(
                  onPressed: (){
                    _startAddNewBooks(context, categoryId);
                    },
                  icon: Icon(Icons.add)),
            label: 'Add',
          ),
        ],
      ),
    );
  }
}
/***///////////*****************

class Book{

  final String id;
  final String title;
  List<String> categories;

  Book({
    required this.id,
    required this.title,
    required this.categories
});

}

class Category {
  final String id;
  final String title;
  final Color color;
  const Category(
      {required this.id, required this.title, this.color = Colors.orange});
}


