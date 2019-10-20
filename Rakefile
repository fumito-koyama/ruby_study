require 'csv'
require 'time'

desc 'study start'
task :start do
  if File.exists? time_file
    puts 'すでに学習を始めています'
    return
  end

  time = Time.now
  File.open(time_file, 'w') do |file|
    file.puts time.to_s
    puts "[#{time.strftime("%R")}]学習を開始しました"
  end
end

desc 'study finish'
task :finish do
  unless File.exists? time_file
    puts '学習を開始していません'
    return
  end

  finished_time = Time.now
  puts "[#{finished_time.strftime("%R")}]学習を終了しました"
  started_time = File.open(time_file).read
  File.delete(time_file)

  started_time = to_time_object(started_time)

  study_minutes = (finished_time - started_time).to_i / 60

  puts "今回の学習時間は#{study_minutes}分です"

  puts '学習内容を記入してください'
  study_content = STDIN.gets.chomp

  CSV.open(history_file, 'a') do |csv|
    csv << [started_time.strftime('%Y-%m-%d %H:%M'),
            finished_time.strftime('%Y-%m-%d %H:%M'),
            study_minutes, study_content]
  end
end

desc 'output total time'
task :total do
  t = 0
  CSV.read(history_file).each{|row| t += row[2].to_i}
  puts "現在までの合計の学習時間は#{t/60}時間#{t%60}分です"
end

def to_time_object(time)
  Time.parse(time)
end

def time_file
  @time_file ||= File.expand_path('time.txt', __dir__)
end

def history_file
  @history_file ||= File.expand_path('history.csv', __dir__)
end
