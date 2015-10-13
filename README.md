# Git, shell & Vim tips & tricks

Script for [Haxorz Day][1] at Selleo.

## Git

### Ignoring User Files

`.gitignore` is good for ignoring files shared among (almost) all developers. It
cannot, however, filter out temporary, work–in–progress, or ‘private’ files.

`.git/info/exclude` is for that purpose.

Before git repo is initiated.

```bash
➜  haxorz_10.15  ls
asciinema_screencasts notes
```

Initiating git repo.

```bash
➜  haxorz_10.15 git:(master) ✗ git s
?? asciinema_screencasts/
?? notes/
```

I don’t want to push `notes/` and other ‘WIP’ files.

```bash
➜  haxorz_10.15 git:(master) ✗ echo 'notes/' >> .git/info/exclude
➜  haxorz_10.15 git:(master) ✗ git s
?? asciinema_screencasts/
```

Now `asciinema_screencasts` can be pushed.


### Opening previous revision(s) of a file

If you use `Vim`, consider using [Extradite commit browser][2].
For ‘vanilla’ Git, use this:

```bash
git show head~1:app/assets/javascripts/file.js
```

Modify `~1` to get previous revisions.  You don’t have to start from `head`. Any
valid Git hash like `e051eff~5` can be used.

You can even checkout that file:

```bash
git checkout e051eff~1 app/assets/javascripts/file.js
```

### Opening Github pull request with hub

`hub pull-request` and you don’t have to open Github page.

See [git pull-request][3] documentation for more info.


## shell / zsh

### Repeating last arguments in shell

You don’t have to do ‘copy–pasta’ to use last command arguments in another
command.

`asciinema_screencasts` can be used in another command.

```bash
➜  haxorz_10.15 git:(master) ✗ ls asciinema_screencasts
01_hx_git_exclude.json
02_hx_git_file_previous_rev_ctrlspace.json
…
```

```bash
# press `ALT-.` after `ls`
ls asciinema_screencasts
```

Some configuration for key bindings in **OS X** may be required.


### bindkey to list keyboard shortcuts in zsh

Use `bindkey` to list key bindings instead of googling how to ‘move forward one
word’ (`ALT-F` if you’re wondering)


## Rails / Pry

### show-doc as an API viewer

`?` alias can be used instead.

```ruby
[1] pry(main)> show-method Array#map

From: array.c (C Method):
Owner: Array
Visibility: public
Number of lines: 13

static VALUE
rb_ary_collect(VALUE ary)
{
    long i;
    VALUE collect;

    RETURN_SIZED_ENUMERATOR(ary, 0, 0, ary_enum_length);
    collect = rb_ary_new2(RARRAY_LEN(ary));
    for (i = 0; i < RARRAY_LEN(ary); i++) {
	rb_ary_push(collect, rb_yield(RARRAY_AREF(ary, i)));
    }
    return collect;
}
```

`show-doc` is not limited to ruby / rails core methods. You can use
`show-method Dog#bark` as well.


### show-method to display any method

`$` alias can be used instead.

```ruby
➜  examples git:(master) ✗ ruby -r pry pig.rb
[1] pry(main)> $ Pig#eat

From: pig.rb @ line 8:
Owner: Pig
Visibility: public
Number of lines: 3

def eat
  "Om nom nom"
end
```

Pig examples taken from [pry documentation][4] ;)

### Debugging with Pry

```ruby
➜  examples git:(master) ✗ ruby -r pry pig.rb
[1] pry(main)> $ Pig#say_hello


def say_hello
  :woof
end
```

‘dog–like’ pig? Let’s fix that!

```ruby
[2] pry(main)> edit Pig#say_hello

def say_hello
  :oink_oink
end
```

Use `-t` to temporary changes (kept in memory)

```ruby
 edit -t Pig#say_hello
```


## Better code examples with asciinema

> Forget screen recording apps and blurry video. Enjoy a lightweight, purely text
> based approach to terminal recording.

[Visit asciinema website][5] for more info.

[![asciicast][6]](https://asciinema.org/a/27763)

asciicast was recorded with:

```bash
asciinema play -w=2 06_hx_debugging_with_pry.json
```

Example taken from [Rubyists, It’s Time to PRY Yourself Off IRB!][7] article on
[sitepoint][8].


## formd to level–up your markdown links formatting

[formd—A Markdown formatting tool][9]

Markdown has two formats for links:

- Inline
- Referenced

Inline: good when you’re creating content. Bad for formatting (80
column–length) and reading.

> The quick brown [fox](http://en.wikipedia.org/wiki/Fox) jumped over

Referenced: good for reading / reviewing text and formatting–friendly.

> the lazy [dog](1)

With `formd` you can easily switch between the two.

[From my `.vimrc`][11]

```VimL
au FileType markdown,text nmap <leader>fr :call Formd("-r")<CR>
au FileType markdown,text nmap <leader>fi :call Formd("-i")<CR>
au FileType markdown,text nmap <leader>ft :call Formd("-f")<CR>
```


[1]: http://selleo.com/blog/geeks-mindset/haxorz-days-in-selleo/
[2]: http://int3.github.io/vim-extradite/
[3]: https://github.com/github/hub#git-pull-request
[4]: https://github.com/pry/pry/wiki/Editor-integration#using-the-edit-command
[5]: https://asciinema.org/
[6]: https://asciinema.org/a/27763.png
[7]: http://www.sitepoint.com/rubyists-time-pry-irb/
[8]: http://www.sitepoint.com/
[9]: http://drbunsen.github.io/formd/
[10]: http://en.wikipedia.org/wiki/Fox
[11]: https://github.com/ryrych/dotfiles/blob/master/vimrc#L237-L239
