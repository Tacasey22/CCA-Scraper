require 'mechanize'
require 'pry'
require 'chronic'
require 'pony'

desc 'scrape the cca sports website and send emails to people'
task :scrape_cca do
  scraper = Mechanize.new
  scraper.user_agent_alias = 'Mac Safari'

  scraper.get('http://www.ccasports.com/leagues/soccer/schedule')

  scraper.page.link_with(text: /Antonia Goalia/).click

  game_rows = scraper.page.at_css('.ccareport').children.search('tr')
  details = []
  game_rows.each do |row|
    details << {}.tap do |hash|
      row.search('td').each do |column|
        unless Chronic.parse(column.text).nil?
          if Chronic.parse(column.text).day == Date.today.day
            hash[:time] = Chronic.parse(column.text).strftime('%l:%M %p')
          else
            hash[:date] = Chronic.parse(column.text).strftime('%b %d, %Y')
          end
        end

        unless column.search('a').empty?
          hash[:enemy] = column.text if column.search('a').first.text != 'Antonia Goalia'
        end
      end
    end
  end

  body = ''
  details.each do |game|
    next if game.empty?
    game.each do |key, value|
      body += "#{key}: #{value} \r\n\r\n"
    end
  end

  Pony.mail(
    to: 'alexoverbeck@gmail.com',
    subject: 'Antonia Goalia',
    body: body,
    via: :smtp,
    via_options: {
      address: 'smtp.sendgrid.net',
      port: '587',
      authentication: :plain,
      user_name: 'app51116270@heroku.com',
      password: 'vkwiafmp7763',
      domain: 'heroku.com',
      enable_starttls_auto: true
    })
end
