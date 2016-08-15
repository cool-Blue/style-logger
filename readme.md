# style-logger

A very basic, isomorphic logger that accepts a style function object that can be used to provide colour output to stdout using [ANSI escape sequences](http://ascii-table.com/ansi-escape-sequences.php) in node or `CSS` style fomatting foe `console.log`.

## Usage

`npm install style-logger`

**_StyleLogger([{WriteStream}[, {object}]])_** optionaly takes a stream and a style functions object and returns a logger

Builds a logger complex including the provided styles and returns a customisable, logger complex including customisation methods.  If no write stream is provided it will log to `stdout`.

```js
    const StyleLogger  = require('style-logger')
    
    function logger(logStream) {
    
        var ESC = '\x1b[', gEND = "m", allOFF = `${ESC}0m`, BOLD = 1, ITALIC = 3, UNDERLINE = 4, IMAGENEGATIVE = 7, FONTDEFAULT = 10, FONT2 = 11, FONT3 = 12, FONT4 = 13, FONT5 = 14, FONT6 = 15, IMAGEPOSITIVE = 27, BLACK = 30, RED = 31, GREEN = 32, YELLOW = 33, BLUE = 34, MAGENTA = 35, CYAN = 36, WHITE = 37, BG_BLACK = 40, BG_RED = 41, BG_GREEN = 42, BG_YELLOW = 43, BG_BLUE = 44, BG_MAGENTA = 45, BG_CYAN = 46, BG_WHITE = 47, CLEAR_SCREEN = `${ESC}2J`;
    
        var ansiStyles = {
            h1: (m, s, e) => `${ESC}${BOLD};${RED}m${m}${allOFF}`,
            h2: (m, s, e) => `${ESC}${BOLD};${BLUE}m${m}${allOFF}`,
            h3: (m, s, e) => `${ESC}${BOLD};${YELLOW}m${m}${allOFF}`
        };
        var cssRed = '#c04848', cssBlue = '#07c', cssYellow = '#ead10e';
        var cssStyles = {
            h1: (m) => [`%c${m}`, `font-weight: bold; color: ${cssRed}`],
            h2: (m) => [`%c${m}`, `font-weight: bold; color: ${cssBlue}`],
            h3: (m) => [`%c${m}`, `font-weight: bold; color: ${cssYellow}`]
        };
    
        return StyleLogger(
            logStream,
            typeof window !== 'undefined' ? cssStyles : ansiStyles,
            {isStart: /.*start/i, isEnd: /.*end/i}
        )
    
    }
     
    // If a logStream is not provided, logs to stdout
    var log = logger();
   
    // returns styled logs...
    log.h1(_message_)
    log.h2(_message_)
    log.h3(_message_)
    
    // or default vanilla
    log(_message_)
    
    // with or without escape sequences (for non-TTY output)
    log.plain();
    log.fancy();
    
    // enable or dissable
    log.om();
    log.off();
    
    // apply a transform to the message before it is wrapped
    log.transform(m => timeStamp() + '\t' + m)
    
    // provide a callback for async operation
    log.h1(_message_, done)  // will call back after write is completed
    
    // also provides events for finnish and errors
    log.onfinish(() => console.log("finished writing to " + this.path || "stdout");
    log.onerror((e) => console.log("error writing to " + this.path || "stdout\n" + e.message);
```


