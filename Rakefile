require 'bundler'
require 'kindle_highlights'
require 'fileutils'
require 'json'
require 'cgi'
require 'mail'
require 'htmlentities'

Mail.defaults do
  delivery_method :smtp, 
    address:              ENV['SMTP'] || "smtp.mailgun.org",
    port:                 ENV['PORT'] || 587,
    user_name:            ENV['USER'], 
    password:             ENV['PASS'],
    authentication:       'plain',
    enable_starttls_auto: true     
end

class Kindle
  def initialize(path = 'data.json')
    @path = path
  end

  def update
    html = HTMLEntities.new
    kindle = KindleHighlights::Client.new(ENV["AMAZON_USER"], ENV["AMAZON_PASS"]) 
    @highlights = []

    kindle.books.each do |key, title|
      kindle.highlights_for(key).each do |highlight|
        highlight["book"] = html.decode(title)
        highlight["highlight"] = html.decode(highlight["highlight"])
        @highlights << highlight
      end
    end
  end

  def save 
    File.open(@path, "w+") do |fp| 
      fp << @highlights.to_json
    end
  end

  def highlights
    @highlights ||= JSON.load(open(@path))
  end

  def random_highlight
    highlights[rand(highlights.length)]
  end
end

task :download do 
  data = Kindle.new
  data.update
  data.save
end

task :print do 
  data = Kindle.new 
  highlight = data.random_highlight
  puts "\"#{highlight["highlight"]}\""
  puts
  puts " -- #{highlight["book"]}, #{highlight["howLongAgo"]}"
  puts
end


task :email do 
  data = Kindle.new
  highlight = data.random_highlight

  mail = Mail.new do
    from    'Kindle Highlights <kindle@highlights.mailgun.com>'
    to      ENV['TO']
    subject "#{Time.now.strftime("%b %d")}: #{highlight["book"]}"
    html_part do
      content_type 'text/html; charset=UTF-8'

      body "<p><i>#{highlight["highlight"]}</i><p><br><p>&mdash; #{highlight["book"]}, #{highlight["howLongAgo"]}</p>"
    end
  end

  mail.deliver
end

task default: [:download, :email]
