in node modules/bin/electron.js paste this

#!/usr/bin/env node

// Use the local Electron installation
var electron = require('electron')

var proc = require('child_process')

// Spawn the Electron process
var child = proc.spawn(electron, process.argv.slice(2), { stdio: 'inherit', windowsHide: false })
child.on('close', function (code) {
  process.exit(code)
})

// Handle termination signals
const handleTerminationSignal = function (signal) {
  process.on(signal, function signalHandler() {
    if (!child.killed) {
      child.kill(signal)
    }
  })
}

handleTerminationSignal('SIGINT')
handleTerminationSignal('SIGTERM')

