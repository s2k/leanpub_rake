# leanpub_rake
Rake tasks helping to check LeanPub manuscripts

* If you have a recent Ruby version installed, you're good to go.
  * Put the Rakefile into the `manuscript` folder of your [LeanPub](https://leanpub.com/) book.
  * You can now run `rake -T` to get a list of the available (two) tasks.
  
Run 

    rake check_linked_files
    
to find issues with files that are referenced in the book, but are missing on the file system.

Run

    rake check_book_file
   
to find files referenced in `Book.txt` that don't exist on the file system.
(If the file `Book.txt` itself is missing, you'll get an exception telling you about this…)