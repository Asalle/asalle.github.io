https://www.gnu.org/software/emacs/manual/html_node/elisp/Defining-Functions.html#Defining-Functions
https://github.com/emacs-mirror/emacs/tree/master/lisp/progmodes
https://google.github.io/flatbuffers/flatbuffers_grammar.html
https://www.emacswiki.org/emacs/ProgMode

I realized that flatbuffers mode is not very similar to cc-mode, e. g. field declaration looks like <fieldname>:<type>, which looks more like rust. After checking out rust-mode it looks like it does not inherit cc-mode and makes its own regex to match those things instead.

Went to the emacs monthly meetup and talked there about my inability to write mode was ~laughed at~ listened out very kindly and given some useful tips.

Took some elisp lessons from Daniel Gopar (https://www.youtube.com/watch?v=CH0RUrO_oww)
https://github.com/Asalle/elisp-bits

This resource is great: https://www.gnu.org/software/emacs/manual/html_node/elisp/Font-Lock-Mode.html

Period matches everything but numbers -solved by matching everything indiscrimitorily, func arg for this exists (docs font-lock-keywords)

grammar: https://google.github.io/flatbuffers/flatbuffers_grammar.html

