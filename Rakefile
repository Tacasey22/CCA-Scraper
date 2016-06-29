require 'mechanize'
require 'pry'
require 'chronic'
require 'pony'

desc 'Scrape CCA website for game times'
task :scrape_cca do
  team = "Antonia Goalia"
  puts "test"
  agent = Mechanize.new
  agent.get('http://www.ccasports.com/leagues/soccer/schedule/')
  agent.page.links_with(text: "Antonia Goalia").first.click
  agent.page.search("table.ccareport tr")
  games=[]
  game_rows = agent.page.search('table.ccareport tr')
  game_rows.each do |row|
    game = {}
    columns = row.search('td')

    columns.each do |column|
      date = Chronic.parse(column.text)
      unless date.nil?
        if date.to_date.to_s == Date.today.to_s
          game[:time]=date.strftime('%l:%M %p')
        else
          game[:date]=date.strftime('%b %d, %Y')
        end
      end

      unless column.search('a').empty?
        game[:enemy] = column.text if column.search('a').first.text != team
      end
    end

    games << game unless game.empty?
  end
  binding.pry
end
