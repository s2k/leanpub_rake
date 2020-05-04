# frozen_string_literal: true

require 'set'

def markup_filenames(manuscript_folder: 'manuscript')
  book_filename = File.join(manuscript_folder, 'Book.txt')
  chapter_files = File.readlines(book_filename).map(&:strip)
  chapter_files.map { |c_fn| File.join(manuscript_folder, c_fn) }
end

def for_each_markup_file
  markup_filenames.each_with_object(Set.new) do |fn, result_set|
    yield fn, result_set
  end
end

def id_definitions
  for_each_markup_file do |fn, id_defs|
    content = File.read(fn)
    res = content.scan(/{\s*id:\s*([-_0-9a-zA-Z]+).*}/).flatten
    id_defs.merge res
    res = content.scan(/{\s*#([-_0-9a-zA-Z]+)\s*.*}/).flatten
    id_defs.merge res
    id_defs
  end
end

def cross_links
  for_each_markup_file do |fn, id_defs|
    content = File.read(fn)
    res = content.scan(/\[[^\]]+\]\(#([-_0-9a-zA-Z]+)\)/).flatten
    id_defs.merge res
    id_defs
  end
end

def unused_ids
  cross_links - id_definitions
end

def missing_ids
  id_definitions - cross_links
end

task default: %i[check_book_file check_references]

desc 'Check file references (lines containing <<[…some text…](a-file-path)'
task :check_references do
  if unused_ids.empty?
    puts 'No unused IDs found'
  else
    puts 'Unused IDs:'
    puts unused_ids.to_a
  end

  if missing_ids.empty?
    puts 'No missing IDs found'
  else
    puts 'Missing IDs:'
    puts missing_ids.to_a
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
    puts 'Book.txt is looking good'
  end
end
