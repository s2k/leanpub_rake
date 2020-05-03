require 'ap'

def missing_reference(md, md_filename, line)
  { missing_file: md[2], referenced_in: md_filename, line: line + 1 }
end

def markup_filenames(manuscript_folder: 'manuscript')
  book_filename = File.join(manuscript_folder, 'Book.txt')
  chapter_files = File.readlines(book_filename).map(&:strip)
  chapter_files.map { |c_fn| File.join(manuscript_folder,c_fn) }
end

task default: %i[check_book_file check_linked_files]

desc 'Check file references (lines containing <<[…some text…](a-file-path)'
task :check_linked_files do
  missing_references = markup_filenames.inject([]) do |res, md_filename|
    unless File.exist?(md_filename) || md_filename.match?(/\a\s*#\w+/)
      puts "Ignore file: '#{md_filename}'"
      next res
    end
    File.foreach(md_filename).with_index do |line, line_no|
      if md = line.match(/<<(\[[^\]]+\])?\((.+?)\)/)
        res << missing_reference(md, md_filename, line_no) unless File.exist?(md[2])
      end
    end
    res
  end

  if missing_references.empty?
    puts 'File name References looking good'
  else
    puts 'Missing file name references:'
    ap missing_references
    exit 1
  end
end

desc 'check whether all files in Boot.txt exist'
task :check_book_file do
  failed = false
  markup_filenames.each do |fn|
    if fn.match?(/^\s*#/)
      puts "Ignoring #{fn}"
      next
    elsif File.exist?(fn)
      next
    else
      puts "ERROR: Missing file #{fn}"
      failed = true
    end
  end
  if failed
    exit 1
  else
    puts 'Looking good'
  end
end
