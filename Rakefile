# Resource：
# - [Rake Tutorial](http://jasonseifer.com/2010/04/06/rake-tutorial)

# Example 1 - Dependencies
directory 'tmp'
file 'tmp/hello.tmp' => 'tmp' do
  `echo 'hello' > 'tmp/hello.tmp'`
end
# 执行：
# > rake 'tmp/hello.tmp'
# 输出：
# 得到 tmp/hello.tmp 文件，且文件内容为 'hello'
# 执行逻辑：
# file 表明后面的参数代表一个文件。首先检查 'tmp/hello.tmp' 这个文件是否存在，
# 如果存在则中断执行，如果不存在，则先去执行它的依赖。
# 它的依赖是 'tmp'，'tmp' 在前面被定义成了一个文件夹，如果文件夹存在，则跳过，否则建立此文件夹。
# 最后向此文件中写入 'hello' 字符串

##########################################

# Example 2 - Running Other Tasks
task :turn_off_alarm do
  puts "Turned off alarm. Would have liked 5 more minutes, though."
end

task :groom_myself do
  puts "Brushed teeth."
  puts "Showered."
  puts "Shaved."
end

task :make_coffee do
  cups = ENV["COFFEE_CUPS"] || 2
  puts "Made #{cups} cups of coffee. Shakes are gone."
end

task :walk_dog do
  puts "Dog walked."
end

task :ready_for_the_day => [:turn_off_alarm, :groom_myself, :make_coffee, :walk_dog] do
  puts "Ready for the day!"
end
# 执行：
# > rake ready_for_the_day
# 输出：
# Turned off alarm. Would have liked 5 more minutes, though.
# Brushed teeth.
# Showered.
# Shaved.
# Made 2 cups of coffee. Shakes are gone.
# Dog walked.
# Ready for the day!

# 通过环境变量传递参数
# > rake COFFEE_CUPS=5 make_coffee
# Made 5 cups of coffee. Shakes are gone.
# (突然明白了在 bash shell 中给一个变量赋值时为什么中间不能有空格，你要想象成这条语句是在 bash 中一条一条执行的，如果有了空格，相当于变成了三个单独的参数，就如上面 'COFFEE_CUPS=5' 有了空格，就变成了 'rake COFFEE_CUPS = 5 make_coffee'。)

##########################################

# Example 3 - Namespaces
namespace :morning do
  task :turn_off_alarm do
    puts "Turned off alarm. Would have liked 5 more minutes, though."
  end

  task :groom_myself do
    puts "Brushed teeth."
    puts "Showered."
    puts "Shaved."
  end

  task :make_coffee do
    cups = ENV["COFFEE_CUPS"] || 2
    puts "Made #{cups} cups of coffee. Shakes are gone."
  end

  task :walk_dog do
    puts "Dog walked."
  end

  task :ready_for_the_day => [:turn_off_alarm, :groom_myself, :make_coffee, :walk_dog] do
    puts "Ready for the day!"
  end
end
# 执行：
# > rake COFFEE_CUPS=3 morning:ready_for_the_day
# 输出：
# Turned off alarm. Would have liked 5 more minutes, though.
# Brushed teeth.
# Showered.
# Shaved.
# Made 3 cups of coffee. Shakes are gone.
# Dog walked.
# Ready for the day!

##########################################

# Example 4 - The Default Task
task :default => 'morning:turn_off_alarm'
# 执行 rake 时如果不加任何参数，则执行 default 任务。
# 执行：
# > rake
# 输出：
# Turned off alarm. Would have liked 5 more minutes, though.

##########################################

# Example 5 - Describing Your Tasks
desc 'sleep'
task :sleep do
  puts 'Go to sleep'
end
# 执行 rake -T 显示有 desc 的 tasks 及它们的 desc
# 执行 rake -AT 显示所有 tasks
# 执行：
# > rake -T
# 输出：
# rake sleep  # sleep

##########################################

# Example 6 - Redefining Tasks
namespace :morning do
  task :groom_myself do
    puts "Styled hair."
  end
end
# 执行：
# > rake morning:groom_myself
# 输出：
# Brushed teeth.
# Showered.
# Shaved.
# Styled hair.

##########################################

# Example 7 - Invoking Tasks
namespace :afternoon do
  task :make_coffee do
    Rake::Task['morning:make_coffee'].invoke
    puts 'Ready for the rest of the day!'
  end
end
# 执行：
# > rake afternoon:make_coffee COFFEE_CUPS=3
# 输出：
# Made 3 cups of coffee. Shakes are gone.
# Ready for the rest of the day!

##########################################

# Example 8 - Real Life Code in Rails
# lib/tasks/tasks.rake
namespace :accounts do
  desc "Email expiring accounts to let them know"
  task :email_expiring => :environment do
    date = ENV['from'] ? Date.parse(ENV['from']) : Date.today
    Account.notify_expiring(date)
  end
end
# :environment task 来用设置 rails 的环境变量，
# 比如在 staging 模式运行 rake accounts:email_expiring，
# 相当于 rake RAILS_ENV=staging accounts:email_expiring

##########################################

# Example 9 - Scheduling Rake Tasks
# 使用 cron 周期执行某个 rake task
# 一个示例：
# 15 * * * * cd /data/my_app/current && bin/rake RAILS_ENV=production accounts:email_expiring

##########################################

# Example 10 - Misc
# Rake.original_dir 显示 rake task 的路径
task :original_dir do
  puts Rake.original_dir
end
# 执行：
# > rake original_dir
# 输出：
# ---/MyCodes/rake-study

##########################################

# Resource：
# - [Rakefile Format](https://ruby.github.io/rake/doc/rakefile_rdoc.html)

# Example - Tasks that Expect Parameters and Have Prerequisites
task :pre_name do
  puts 'Pre name'
end

task :name, [:first_name, :last_name] => [:pre_name] do |t, args|
  puts t.name
  puts t.prerequisites
  args.with_defaults(:first_name => "John", :last_name => "Dough")
  puts "First name is #{args.first_name}"
  puts "Last  name is #{args.last_name}"
end
# 第一个参数 t 表示 name 这个 task 自己，它的属性有 t.name、t.prerequisites 等
# 执行：
# > rake "name[billy bob, smith]"
# 输出：
# Pre name
# name
# pre_name
# First name is billy bob
# Last  name is smith
