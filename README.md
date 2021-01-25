These steps and configurations are taken from https://github.com/garageScript/curriculum/wiki/Vim-Config

# SYSTEM INFO
* For OSX users:
  * Make sure you are using [iterm2](https://iterm2.com/). This terminal supports true colors, which renders the UI more clearly and is better for your eyes. The default mac terminal does not support true colors.
* Update vim
  * `sudo add-apt-repository ppa:jonathonf/vim`
  * `sudo apt update`
  * `sudo apt install vim`
* Make sure you have the following programs installed:
  * [ripgrep](https://github.com/BurntSushi/ripgrep) - used for fast searching
    * Method 1: `sudo apt-get install ripgrep` - This installs `rg` for all users (YAY!)
      * Run `lsb_release -a` to see what ubuntu version you are on.
    * Method 2: Should [install rust first](https://www.rust-lang.org/tools/install) to get the lastest version of ripgrep via `cargo install ripgrep`
      * Also needs `sudo apt install build-essential` to avoid `linker cc` errors during installation.
      * This only installs `rg` in your userspace, so no other users will have this command.
* Install NodeJS. 
  * Pro tip: Install nodejs as root directly, don't install `nvm` on root user.
  * Ubuntu Sudo: `curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -`
  * All else: Install [nvm](https://github.com/nvm-sh/nvm) then run `nvm i 14` (replace 14 with major version)
    * replace `setup_14` with current major version 
# Steps
1. Install [vim plug](https://github.com/junegunn/vim-plug)
2. Open `~/.vimrc` file. Set paste mode `:set paste`
2. Copy / paste the config below to `~/.vimrc`
3. Reload your new configs by running `:source ~/.vimrc` 
4. Run `:PlugInstall`
5. Run `:CocInstall coc-tsserver coc-json coc-html coc-css`
6. Run `:CocInstall coc-eslint coc-prettier`
7. Copy the config below to `~/.vim/coc-settings.json`
8. Close vim and reopen!

# Important Disclaimers
* `leader` key is set to `,` and only usable in normal mode.
* Paste toggle is set to `CTRL-Z`. To paste content without auto indent, type `ctrl-z` to go into paste mode. After pasting, type `ctrl-z` again to go back to nopaste (default) mode.
* Jumping between windows - No need a `CTRL-w` prefix. Just `CTRL-h` to go to left window, `jkl` for the other directions.
* There are no `swp` files. Make sure you `commit` your code frequently (as you should be doing)

# Common usage
* Emmet Plugin
  * Type `h1`. To autocomplete the tag, type `ctrl-Y,` (control y and then ,).
* Leader F - `ctrl-p` to open up file search to navigate between files quickly
  * `ctrl-v` to vertical split the search result

--- 

`~/.vimrc`
```
" Specify a directory for plugins
" - For Neovim: stdpath('data') . '/plugged'
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

" Make sure you use single quotes

" Shorthand notation; fetches https://github.com/junegunn/vim-easy-align
Plug 'junegunn/vim-easy-align'

" Any valid git URL is allowed
Plug 'https://github.com/junegunn/vim-github-dashboard.git'

" On-demand loading
Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle' }

" Using a non-master branch
Plug 'rdnetto/YCM-Generator', { 'branch': 'stable' }

" Plugin outside ~/.vim/plugged with post-update hook
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }

" Unmanaged plugin (manually installed and updated)
Plug '~/my-prototype-plugin'

" For JS Development
Plug 'pangloss/vim-javascript'
Plug 'leafgarland/typescript-vim'
Plug 'peitalin/vim-jsx-typescript'
Plug 'styled-components/vim-styled-components', { 'branch': 'main' }
Plug 'jparise/vim-graphql'
Plug 'mattn/emmet-vim'
Plug 'ctrlpvim/ctrlp.vim'

" COC
" Use release branch (recommend)
Plug 'neoclide/coc.nvim', {'branch': 'release'}
" AFTERWARDS!!!!
" RUN IN VIM----
" :CocInstall coc-tsserver coc-json coc-html coc-css
" :CocInstall coc-eslint coc-prettier

" Color themes
Plug 'itchyny/lightline.vim'
Plug 'joshdick/onedark.vim'

" Initialize plugin system
call plug#end()

" ------------------------------------------------------
" ------------------------------------------------------
" ------------------------------------------------------
" ---------------- START PLUGIN RELATED SECTION ------------

" --- start --- Onedark color scheme
"Use 24-bit (true-color) mode in Vim/Neovim when outside tmux.
"If you're using tmux version 2.2 or later, you can remove the outermost $TMUX check and use tmux's 24-bit color support
"(see < http://sunaku.github.io/tmux-24bit-color.html#usage > for more information.)
if (empty($TMUX))
  if (has("nvim"))
    "For Neovim 0.1.3 and 0.1.4 < https://github.com/neovim/neovim/pull/2198 >
    let $NVIM_TUI_ENABLE_TRUE_COLOR=1
  endif
  "For Neovim > 0.1.5 and Vim > patch 7.4.1799 < https://github.com/vim/vim/commit/61be73bb0f965a895bfb064ea3e55476ac175162 >
  "Based on Vim patch 7.4.1770 (`guicolors` option) < https://github.com/vim/vim/commit/8a633e3427b47286869aa4b96f2bfc1fe65b25cd >
  " < https://github.com/neovim/neovim/wiki/Following-HEAD#20160511 >
  if (has("termguicolors"))
    set termguicolors
  endif
endif

syntax on
colorscheme onedark
" --- END --- Onedark color scheme

" --- start --- NerdTree Settings
map <C-n> :NERDTreeToggle<CR>
" --- END --- NerdTree

" --- start --- COC Settings
" https://github.com/neoclide/coc.nvim/wiki/Completion-with-sources

" use <tab> for trigger completion and navigate to the next complete item
inoremap <silent><expr> <Tab>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<Tab>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~ '\s'
endfunction

" DEBUGGING -
" <tab> could be remapped by another plugin, use :verbose imap <tab> to check if it's mapped as expected.
" --- end --- COC Settings

" --- start --- ctrlp Settings to search for files using ripgrep if available
" https://github.com/BurntSushi/ripgrep
if executable('rg')
  let g:ctrlp_user_command = 'rg %s --files --hidden --color=never --glob=!locales/*'
endif

" Files to ignore
set wildignore+=*/tmp/*,*.so,*.swp,*.zip     " Linux/MacOSX
set wildignore+=*\\tmp\\*,*.swp,*.zip,*.exe  " Windows
" --- end --- ctrlp Settings

" ---------------- END PLUGIN RELATED SECTION ------------
" ------------------------------------------------------
" ------------------------------------------------------
" ------------------------------------------------------


" Show linenumber
set number

" --- start --- Tabs vs Spaces
set expandtab       "Use softtabstop spaces instead of tab characters for indentation
set shiftwidth=2    "Indent by 2 spaces when using >>, <<, == etc.
set softtabstop=2   "Indent by 2 spaces when pressing <TAB>

set autoindent      "Keep indentation from previous line
set smartindent     "Automatically inserts indentation in some cases
set cindent         "Like smartindent, but stricter and more customisable
" --- end --- Tabs vs spaces

" change the mapleader from \ to ,
let mapleader=","

" Key for toggling paste
set pastetoggle=<c-z>

" Allow you to scroll using mouse scroll wheel
set mouse=a

" Source - https://vim.fandom.com/wiki/Switch_between_Vim_window_splits_easily
set wmw=0           " do not display current line of each minimized file
nmap <c-h> <c-w>h
nmap <c-l> <c-w>l
nmap <c-j> <c-w>j
nmap <c-k> <c-w>k

set noswapfile      " No swap files

" enable incremental search in vim
set incsearch
set hlsearch " Highlight all search results
```

`~/.vim/coc-settings.json`
```
{
  "coc.preferences.formatOnSaveFiletypes": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "tsserver.formatOnType": true,
  "coc.preferences.formatOnType": true,
  "javascript.validate.enable": false
}
```
