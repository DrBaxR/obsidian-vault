Tags: #linux 
Created: 2022-11-06 12:11
References: 

# Neovim
Neovim is a text editor that is meant to be a *hyperextensible* extension of [[Vim]].

It can be configured in the traditional way, using [[Vimscript]], or you can use [[Lua]] in order to achieve the same results in a more popular scripting language. In order to configure Neovim with lua, you need to create a `~/.config/nvim/init.lua` where you can include all your config.

**Note:** A much neater and more organized way to configure Neovim is to split your config in multiple files and then require them in the main init file.