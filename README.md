# reproducible-research-ideas
I'll use this repo to dump ideas about reproducible research.

## My current workflow for simulations

1. Write a text input file for a given simulation program (e.g Gaussian/GAMESS/Nwchem input files), or use a GUI to produce a binary or text file (for example, Abaqus CAE).
2. Run the program
3. Take the textual output directly or use a script to extract relevant data from the output (which can also be binary)
4. Process data to create a plot, only with experimental points or also with a fitting function; in some cases, compare multiple data coming from multiple input files
5. Tweak code in order to get a better image
6. Include image in .latex source
7. Compile LaTeX to PDF.

### Problems

1. Not knowing exactly the kind of simulations and the typical result, input and output files got scattered in the filesystem, without any kind of order.
2. A versioning system was used for the LaTeX source but not for input and output files, causing uncertainty about the "update status" of graphs (I didn't know if a simulation re-run was needed)
3. Some team members wrote text in Microsoft Word instead of writing LaTeX in a simple editor, causing various problems of formatting during the copy/paste; this led to a good amount of time loss
4. Multiple simulations were run with customs scripts, in foreground, with no control over the process (for example it was impossible to stop a single job without stopping everything and forcing to restart **every** simulations, even those already successfully run; if a job failed, there wasn't a clear indication)
5. Time constraints put so much pressure that code duplication happened (for graphs creation/manipulation) 
6. Libraries/programs versions were not tracked
6. Matplotlib didn't provide reasonable defaults, thus requiring constant tweaking, worsened by code duplication
7. Code was entangled: there wasn't a clear separation between presentation code (graphical tweaks) and logic
8. The team chose to produce color images, but it would have been nice to produce **both** B/W and color
9. Some code was provided in appendix, but that's not nearly enough to make simulations reproducible
10. GIT history was messy due to the lack of squashing
11. PDF compilation became slow because ```\input{}``` was used instead of ```\include{}```

## Workflow proposal (WIP)

1. Use a queue software like Grid Engine (GE). GE can be easily installed on ubuntu via repositories, an installation from source code is discouraged (it's a mess of scripts)
2. If needed, configure a Beowulf cluster (I already did that for fun)
3. If the beowulf cluster is too slow, request resources to the professor for time on a university server/cluster. In any case, use a script to automate the upload and the job submission
4. Use GIT for everything (this was easy)
5. Explore literate programming (e.g Pweave)
6. Use tools like vagrant to guarantee reproducible environments
6. Use HDF5 to save simulations output as an intermediate step. Therefore we would save
  1. input files
  2. output files
  3. HDF file containing results from output (obviously produced by a script)
7. Find a way to separate code regarding visual tweaking from "logic code". In addition, clearly separate the various steps (output to HDF5 processing, HDF5 processing). Write functions that accept an ```Experiment``` and produce a simple graph, a graph with fitting, or a function that compares multiple experiments
8. Investigate the easiest way to incorporate multiple subimages (it could be a single image with subplots coming from matplotlib, or multiple images stiched together by LaTeX)
