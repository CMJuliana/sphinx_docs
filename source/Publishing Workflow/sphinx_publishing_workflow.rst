.. _publishing_workflow:

=============================
Sphinx Publishing Workflow
=============================

The ``build_and_deploy.yml`` GitHub Actions workflow is triggered automatically whenever new
commits are pushed to the ``master`` branch.

The sections below describe each step of the pipeline.

Install Dependencies
=======================

The workflow begins by installing all Python dependencies required for building the documentation. 

It uses GitHub actions preconfigured Python environment along with the the packages listed in
``requirements.txt``.

.. code-block::

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

Build Sphinx Documentation
=============================

Next, the workflow generates the HTML documentation using Sphinx. 

The project uses the ``Makefile`` included in the repository, so the build process is
reduced to a single command:

.. code-block::

      - name: Build Sphinx Documentation
        run: |
          make html

Deploy to GitHub Pages
========================

Finally, the workflow deploys the generated HTML files to **GitHub Pages**.  

This is done using GitHub's official deployment actions, which handle artifact preparation
and publishing.

.. code-block::

      - name: Setup Pages
        uses: actions/configure-pages@v5
        
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './build/html' 
          
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
