require 'jekyll'
require 'jekyll-contentful-data-import'

desc "Import Contentful Data with Custom Mappers"
task :contentful do
  Jekyll::Commands::Contentful.process([], {}, Jekyll.configuration['contentful'])
end

desc "Generate jekyll posts from contentful data"
task :generate_jekyll_posts do
	require 'yaml'

	Dir.glob('./_data/contentful/spaces/blog/blogPost/*.yaml') do |file|
  		post = YAML.load_file(file)
  		jekyll_filename = "#{post['date'].to_date}-#{post['slug']}.md"

      puts "Generating #{jekyll_filename} from #{file}"

  		# Write the post to a blog file
  		open("./_posts/#{jekyll_filename}", 'w') { |f|
		    f.puts "---"
        f.puts "layout: content"
        f.puts "title: #{post['title']}"
        f.puts "date: #{post['date'].to_date}"
        f.puts "---"
        f.puts post['content']
		  }	
	end
end