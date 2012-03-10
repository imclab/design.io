{spawn, exec}   = require 'child_process'
mint            = require 'mint'
fs              = require 'fs'

task 'coffee', ->
  coffee = spawn './node_modules/coffee-script/bin/coffee', ['-o', 'lib', '-w', 'src']
  coffee.stdout.on 'data', (data) -> console.log data.toString().trim()
  #coffee2 = spawn './node_modules/coffee-script/bin/coffee', ['-w', 'test', '-o', 'test']
  #coffee2.stdout.on 'data', (data) -> console.log data.toString().trim()
  #coffee2.stderr.on 'data', (data) -> console.log data.toString().trim()

task 'build', ->
  result = fs.readFileSync "./src/design.io/client.coffee", "utf-8"
  mint.coffee result, {}, (error, result) ->
    fs.writeFileSync "design.io.js", result
    mint.uglifyjs result, (error, compressed) ->
      fs.writeFile "design.io.min.js", compressed
      
  setTimeout((=>), 1000)