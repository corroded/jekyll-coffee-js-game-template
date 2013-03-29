require 'rubygems'
require 'rake'
require 'haml'

desc 'Pre-compile the files'
task :pre_compile do
  puts "ENV: #{ENV['JEKYLL_ENV']}"

  puts "\nCompiling HAML files..."
  file_list = Dir.glob("**/*.haml")

  file_list.each do |file|
    file_contents = File.open(file, "r").read
    output_file_name = file.gsub(".haml", ".html")
    puts " *   Generating: #{output_file_name}"
    engine = Haml::Engine.new(file_contents)
    output = engine.render
    File.open(output_file_name, "w+") do |f|
      f.write(output)
    end
    puts " * #{output_file_name} generated!"
  end
  puts "HAML files compiled!\n\n"
end

# TODO: Add an auto start for jekyll to compile
desc 'Compile files, commit and deploy to gh-pages'
task :deploy do
  # precompile for production
  Rake::Task["pre_compile"].execute

  # run jekyll command to generate site folder
  `jekyll`

  # copy the _site folder in a tmp folder outside the repo
  `mkdir ~/jekylltmp`
  `cp -r _site/* ~/jekylltmp/`

  # add the image assets (for some reason jekyll-assets doesn't include this automatically)
  `cp -r _assets/images/ ~/jekylltmp/assets/`

  # switch to gh-pages branch
  `git checkout gh-pages`

  # copy contents of _site to gh-pages root (with overrides)
  `cp -rf ~/jekylltmp/* .`

  # add nojekyll file
  `touch .nojekyll`

  # git add + commit
  system "git add ."
  system "git commit"

  # delete the tmp folder from jekyll
  `rm -r ~/jekylltmp`
end

desc 'Compile the files and run the server locally to preview'
task :preview do
  Rake::Task['pre_compile'].execute

  `jekyll`

  `cp -r _assets/ _site/assets/`

  `open _site/index.html`
end

namespace :reload do
  desc 'Refresh compiled coffee files'
  task :js do
    `jekyll`
    `cp -r _assets/ _site/assets/`
  end
end
