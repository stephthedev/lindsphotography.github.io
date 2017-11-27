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

task :generate_images do 
  require 'contentful'
  require 'fileutils'

  client = Contentful::Client.new(
    space: ENV["CONTENTFUL_SPACE_ID"],
    access_token: ENV["CONTENTFUL_ACCESS_TOKEN"]
  )

  assets = client.assets
  yaml = Array.new
  assets.each do | asset |
    image = Hash.new
    image['url'] = asset.url
    image['thumb_url'] = asset.url(format: 'jpg', fit: "thumb", width: 200, height: 200)
    image['title'] = asset.fields[:title]
    image['description'] = asset.fields[:description]
    yaml.push(image)
  end

  output_dir = "./_data/contentful/spaces/blog/assets"
  FileUtils::mkdir_p(output_dir)

  # Write the post to a blog file
  open("#{output_dir}/assets.yaml", 'w') { |f|
    f.puts yaml.to_yaml
  }
end