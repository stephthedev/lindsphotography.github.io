require 'jekyll'
require 'jekyll-contentful-data-import'

namespace :contentful do
  desc "Import contentful blog posts"
  task :content do
    Jekyll::Commands::Contentful.process([], {}, Jekyll.configuration['contentful'])
  end

  desc "Generate jekyll posts from contentful data"
  task :process_posts do
  	require 'yaml'
    require 'fileutils'

    output_dir = "./_posts"
    FileUtils::mkdir_p(output_dir)

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

  task :assets do 
    require 'contentful'
    require 'fileutils'

    client = Contentful::Client.new(
      space: ENV["CONTENTFUL_SPACE_ID"],
      access_token: ENV["CONTENTFUL_ACCESS_TOKEN"]
    )
    assets = client.assets

    yaml = Array.new
    assets.each do | asset |
      puts "Processing image #{asset.fields[:file].file_name} (#{asset.fields[:title]})"
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

  desc 'This rebuilds development db'
  task :all => ["contentful:content", "contentful:process_posts", "contentful:assets"]
end