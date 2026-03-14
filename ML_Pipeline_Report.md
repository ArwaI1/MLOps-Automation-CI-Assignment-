# Report: Fixing the ML Pipeline File

Here are all the detailed bugs found in the original `.github/workflows/ml-pipeline.yml` file and exactly how they were fixed.

### Bug 1: Wrong Branch Setting (Wrong words and missing dash)
- **What was wrong:** Under the `on: push:` section, the original file had the word `branches: main`. This tells GitHub to run the pipeline *only* when pushing to the `main` branch. But the assignment asked to run on all branches *except* `main`.
- **How it was fixed:** 
  1. I changed the word `branches:` to `branches-ignore:`. 
  2. On the next line, I added a dash and a space before the word main, like this: `- main`.

### Bug 2: Missing Checkout Action (Missing lines and code directory)
- **What was wrong:** Under the `steps:` section, the original file jumped straight to setting up Python and installing. It forgot to bring (or "checkout") the actual code from the repository into the pipeline runner. Without this, the pipeline has no files to work with and wouldn't even find `requirements.txt`.
- **How it was fixed:** I added a whole new step as the very first item under `steps:`. I added these exact missing lines:
  ```yaml
      - name: Checkout Code
        uses: actions/checkout@v4
  ```
  *(Note: The dash `-` before `name` means it is a list item for the steps).*

### Bug 3: Missing Linter Command (Missing a whole line)
- **What was wrong:** The step named `- name: Linter Check` was completely empty. It didn't have the action command telling the computer what to actually run.
- **How it was fixed:** I added a new line right under it with the command to run a Python linter. I added this exact missing line:
  ```yaml
        run: flake8 .
  ```

### Bug 4: Wrong Spacing/Indentation (Missing extra spaces)
- **What was wrong:** In the step named `- name: Model Dry Test`, there was a `run: |` line. The next line with the python command (`python -c "import torch; print('Model environment ready!')"`) started exactly under the word `run`. In YAML files, lines under `run: |` must be pushed to the right (indented) with extra spaces.
- **How it was fixed:** I moved the `python -c` line to the right by adding spaces before it, so it is properly indented under the `run` block.

### Bug 5: Missing Final Step to Save README (Missing action at the end)
- **What was wrong:** The assignment asked for a final step to upload the `README.md` file as a document artifact named `project-doc`, but this whole step was missing at the end of the file.
- **How it was fixed:** I added a brand new action block at the very bottom of the file with these missing lines:
  ```yaml
      - name: Upload README Artifact
        uses: actions/upload-artifact@v4
        with:
          name: project-doc
          path: README.md
  ```

---

## Required Deliverables

**1. My GitHub Public Repository Link:**
[Insert your GitHub link here]

**2. Screenshot of Successful Run:**
![Insert picture of green Action tab here]
