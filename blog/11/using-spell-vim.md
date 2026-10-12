

# Using spell with VIM

I really like VIM and I don't want to use another editor to write my LaTeX files. A very useful feature to help out with article writing is to use spell for orthographic checking with VIM. Just use the following command:

    :setlocal spell spelllang=pt_br

Just remember to replace "pt\_br" by your locale. Spell will then highlight your errors. Moreover, as VIM recognizes LaTeX syntax it doesn't apply spell check on LaTeX's keywords.
