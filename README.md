﻿# monte-carlo-rng
<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/pyodide/v0.21.3/full/pyodide.js"></script>
  </head>

  <body>
    <p>
      You can execute any Python code. Just enter something in the box below and
      click the button.
    </p>
    <input id="code" value="sum([1, 2, 3, 4, 5])" />
    <button onclick="evaluatePython()">Run</button>
    <br />
    <br />
    <div>Output:</div>
    <textarea id="output" style="width: 100%;" rows="6" disabled></textarea>

    <script>
      const output = document.getElementById("output");
      const code = document.getElementById("code");

      function addToOutput(s) {
        output.value += ">>>" + code.value + "\n" + s + "\n";
      }

      output.value = "Initializing...\n";
      // init Pyodide
      async function main() {
        let pyodide = await loadPyodide();
        output.value += "Ready!\n";
        return pyodide;
      }
      let pyodideReadyPromise = main();

      async function evaluatePython() {
        let pyodide = await pyodideReadyPromise;
        try {
          let output = pyodide.runPython(code.value);
          addToOutput(output);
        } catch (err) {
          addToOutput(err);
        }
      }
    </script>
  </body>
</html>
