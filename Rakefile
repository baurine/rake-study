# Resource：http://jasonseifer.com/2010/04/06/rake-tutorial

# Example 1 - Dependencies
directory 'tmp'
file 'tmp/hello.tmp' => 'tmp' do
  `echo 'hello' > 'tmp/hello.tmp'`
end
# 执行：rake 'tmp/hello.tmp'
# 执行逻辑：
# file 表明后面的参数代表一个文件。首先检查 'tmp/hello.tmp' 这个文件是否存在，
# 如果存在则中断执行，如果不存在，则先去执行它的依赖。
# 它的依赖是 'tmp'，'tmp' 在前面被定义成了一个文件夹，如果文件夹存在，则跳过，否则建立此文件夹。
# 最后向此文件中写入 'hello' 字符串

