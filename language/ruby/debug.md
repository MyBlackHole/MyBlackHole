# debug

```shell
sudo gem install byebug
sudo byebug ./bin/emulator -r store -p 80


(byebug) help

  break      -- Sets breakpoints in the source code
  catch      -- Handles exception catchpoints
  condition  -- Sets conditions on breakpoints
  continue   -- Runs until program ends, hits a breakpoint or reaches a line
  debug      -- Spawns a subdebugger
  delete     -- Deletes breakpoints
  disable    -- Disables breakpoints or displays
  display    -- Evaluates expressions every time the debugger stops
  down       -- Moves to a lower frame in the stack trace
  edit       -- Edits source files
  enable     -- Enables breakpoints or displays
  finish     -- Runs the program until frame returns
  frame      -- Moves to a frame in the call stack
  help       -- Helps you using byebug
  history    -- Shows byebug's history of commands
  info       -- Shows several informations about the program being debugged
  interrupt  -- Interrupts the program
  irb        -- Starts an IRB session
  kill       -- Sends a signal to the current process
  list       -- Lists lines of source code
  method     -- Shows methods of an object, class or module
  next       -- Runs one or more lines of code
  pry        -- Starts a Pry session
  quit       -- Exits byebug
  restart    -- Restarts the debugged program
  save       -- Saves current byebug session to a file
  set        -- Modifies byebug settings
  show       -- Shows byebug settings
  skip       -- Runs until the next breakpoint as long as it is different from the current one
  source     -- Restores a previously saved byebug session
  step       -- Steps into blocks or methods one or more times
  thread     -- Commands to manipulate threads
  tracevar   -- Enables tracing of a global variable
  undisplay  -- Stops displaying all or some expressions when program stops
  untracevar -- Stops tracing a global variable
  up         -- Moves to a higher frame in the stack trace
  var        -- Shows variables and its values
  where      -- Displays the backtrace

(byebug) break Object.put_object
Created breakpoint 1 at Object:put_object
(byebug) break Servlet:do_PUT
*** Invalid breakpoint location

(byebug) break server.rb:77
*** No file named server.rb

(byebug) break lib/emulator/server.rb:77
Created breakpoint 2 at /media/black/Data/Documents/github/Ruby/oss-emulator/lib/emulator/server.rb:77
(byebug) break lib/emulator/server.rb:113
Created breakpoint 3 at /media/black/Data/Documents/github/Ruby/oss-emulator/lib/emulator/server.rb:113
(byebug) break lib/emulator/server.rb:35
Created breakpoint 4 at /media/black/Data/Documents/github/Ruby/oss-emulator/lib/emulator/server.rb:35
(byebug) break lib/emulator/server.rb:21
Created breakpoint 5 at /media/black/Data/Documents/github/Ruby/oss-emulator/lib/emulator/server.rb:21
(byebug) c
```
