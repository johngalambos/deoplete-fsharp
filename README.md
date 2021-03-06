[![MIT-LICENSE](http://img.shields.io/badge/license-MIT-blue.svg?style=flat)](https://github.com/callmekohei/deoplete-fsharp/blob/master/LICENSE)


[![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg)](https://gitter.im/fsugjp/public)


![alt text](./pic/sample.gif)

# deoplete-fsharp

F# support for Vim / Neovim

using [deopletefs](https://github.com/callmekohei/deopletefs)

<br>
<br>


## Auto-completion

![alt text](./pic/deoplete2.png)

## Installing

deoplete-fsharp requires mono and FSharp installed.

installing with dein.vim

init.vim
```vim
call dein#add('Shougo/deoplete.nvim')
```
dein.toml
```toml
[[plugins]]
repo = 'callmekohei/deoplete-fsharp'
hook_post_update = '''
    if dein#util#_is_windows()
      let cmd = 'install.cmd'
    else
      let cmd = 'bash install.bash'
    endif
    let g:dein#plugin.build = cmd
'''
```
add if you use vim8
```toml
[[plugins]]
repo = 'roxma/nvim-yarp'

[[plugins]]
repo = 'roxma/vim-hug-neovim-rpc'
```

## Configuration
```vim
let g:deoplete#enable_at_startup = 1
```

<br>
<br>

## How to Run


![alt text](./pic/quickrun2.png)


( require plugins )
```
thinca/vim-quickrun
Shougo/vimproc.vim
```

( dein.toml setting )
```toml

[[plugins]]
repo = 'Shougo/vimproc.vim'
build = 'make'

[[plugins]]
repo = 'thinca/vim-quickrun'
hook_add = '''
    set splitright
    let g:quickrun_config = {
    \
    \     '_' : {
    \           'runner'                          : 'vimproc'
    \         , 'runner/vimproc/updatetime'       : 60
    \         , 'hook/time/enable'                : 1
    \         , 'hook/time/format'                : "\n*** time : %g s ***"
    \         , 'hook/time/dest'                  : ''
    \         , "outputter/buffer/split"          : 'vertical'
    \         , 'outputter/buffer/close_on_empty' : 1
    \     }
    \
    \     , 'fsharp' : {
    \           'command'                         : 'fsharpi --readline-'
    \         , 'tempfile'                        : '%{tempname()}.fsx'
    \         , 'runner'                          : 'concurrent_process'
    \         , 'runner/concurrent_process/load'  : '#load "%S:gs?\\?/?";;'
    \         , 'runner/concurrent_process/prompt': '> '
    \     }
    \ }
'''
```

like this if you use window's Neovim.
```
'command': 'mono "C:\Program Files\Mono\lib\mono\fsharp\fsi.exe" --readline-'
```


( How to run ( QuickRun ) )
```
: w
: QuickRun
```

<br>
<br>

## How to Test

![alt text](./pic/persimmon2.png)

nuget `Persimmon.Script`
```
nuget Persimmon.Script
```
code following
```fsharp
/// require Persimmon libraries and open modules.
#r "./packages/Persimmon/lib/net45/Persimmon.dll"
#r "./packages/Persimmon.Runner/lib/net40/Persimmon.Runner.dll"
#r "./packages/Persimmon.Script/lib/net45/Persimmon.Script.dll"

open Persimmon
open UseTestNameByReflection
open System.Reflection

/// write your test code here.
let ``a unit test`` = test {
  do! assertEquals 1 2
}

/// print out test report.
new Persimmon.ScriptContext()
|> FSI.collectAndRun( fun _ -> Assembly.GetExecutingAssembly() )

```
do test
```
: w
: QuickRun
```
