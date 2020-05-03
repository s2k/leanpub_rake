# leanpub_rake

Rake tasks helping to check LeanPub manuscripts

* If you have a recent Ruby version (2.5.8, 2.6.6, 2.7.1 seem to work) installed, you're good to go.
* Put the Rakefile into the `root` folder of your [LeanPub](https://leanpub.com/) book.
  (on the same level as the `manuscript` folder)
* You can now run `rake -T` to get a list of the available (two) tasks.
  
Run 

    rake check_references
    
to find issues with cross references, i.e. references to other parts of the book (that _should_ have IDs defined for them).

Run

    rake check_book_file
   
to find files referenced in `Book.txt` that don't exist on the file system.
(If the file `Book.txt` itself is missing, you'll get an exception telling you about this…)
