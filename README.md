<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdn.jsdelivr.net/pyodide/v0.21.3/full/pyodide.js"></script>
    Monte Carlo Simulation for normally distributed random variables
  </head>

  <body>
    <p>
      This allows for n simulations of the multiplication of two random vaiables with known tolerances and cpks.  This version is under construction 9/18/22 JRL
    <br />
    <br />
     Variable #1  
    <br />
    <input id="code" value="enter mean" />
    <br />
    <br />
    <input id="code" value="enter LSL" />
    <br />
    <br />
    <input id="code" value="enter USL" />
    <br />
    <br />
    <input id="code" value="enter CPK" />
    <br />
    <br />
    Variable #2
    <br />
    <input id="code" value="enter mean" />
    <br />
    <br />
    <input id="code" value="enter LSL" />
    <br />
    <br />
    <input id="code" value="enter USL" />
    <br />
    <br />
    <input id="code" value="enter CPK" />
    <br />
    <br />
    <button onclick="evaluatePython()">Run</button>
    <br />
    <br />
    </p>
    <div>Output:</div>
    <textarea id="output" style="width: 100%;" rows="6" disabled></textarea>

    <script>
      const output = document.getElementById("output");
      const code = document.getElementById("code");
      var codeval = code;

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
        await pyodide.loadPackage("numpy");
       
         
        try {
          let output = pyodide.runPython(`
          import js
          from js import codeval
          print(codeval)
          codeval
          mean = codeval.to_py()
          mean
          print(mean)
          #import numpy as np
          #np.random.normal(mean, 0.025,100)
          `);
          
          let data = "hello world!";
		  pyodide.FS.writeFile("/hello.txt", data, { encoding: "utf8" });
		  pyodide.runPython(`
with open("/hello.txt", "r") as fh:
	data = fh.read()
	print(data)
`); 
          addToOutput(output);
        } catch (err) {
          addToOutput(err);
        }
      }
      main()
    </script>
    <a href="hello.txt" download="hello.txt">Download the txt</a>
  </body>
</html>
