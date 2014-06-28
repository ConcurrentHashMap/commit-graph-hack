## commit-graph-hack

Simple Ruby and bash script that will enhance your commit history in your GitHub profile commit graph.

### How does it work?

The following Ruby code will generates a bash script that then will create random numbers of commits each day for the period. It's a bit hacky but... it works!

```ruby
require 'active_support/time'

class CommitGraphHack
  # the period for which the data should be created (as time ago from now)
  period = 12.month

  start = Time.now - period

  length = period / 86400
  
  commits = length.times.map { Random.rand(10) }

  randomness = {0 => 0, 1 => 0, 2 => 0, 3 => 1, 4 => 2, 5 => 3, 6 => 6, 7 => 0, 8 => 0, 9 => 1, 10 => 0}

  File.open('./generate-commits.sh', 'w') do |f|
    commits.each do |commit|
      randomness[commit].times do
        f.puts "echo \"#{Random.new_seed}\" > ./temp"
        f.puts "git add ./temp"
        f.puts "GIT_AUTHOR_DATE='#{start.to_s[0..9]}T12:00:00' GIT_COMMITTER_DATE='#{start.to_s[0..9]}T12:00:00' \\"
        f.puts "git commit -a -m 'Make the commit graph burn :fire:' --date='#{start.to_s[0..9]}T12:00:00'"
      end
      start += 86400
    end
    f.puts "rm ./temp"
    f.puts "git rm ./temp"
    f.puts "git commit -a -m 'Made some rocket science :rocket:'"
  end
end
```
To apply it to any repository you like, simply copy the generated bash script into your repository, run it and remove the `temp` file afterwards.

### Heavily inspired by
  - http://bd808.com/blog/2013/04/17/hacking-github-contributions-graph/
  - https://github.com/bd808/profile-life
  - https://kivikakk.ee/2013/05/07/contrigraph.html
  - https://github.com/will
  - https://github.com/contrigraph
  - https://github.com/gelstudios/gitfiti

### License
Licensed under MIT License