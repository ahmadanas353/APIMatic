.. APIMatic Assignment Tasks documentation master file, created by
   sphinx-quickstart on Wed Feb 26 21:27:22 2025.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

APIMatic’s Docs as Code: A Quick Start Guide
============================================

Welcome to APIMatic’s Docs as Code—a modern, developer-friendly approach to creating, managing, and publishing API documentation. This guide will help you get started quickly and showcase how APIMatic can streamline your documentation workflow, improve collaboration, and ensure consistency across your API ecosystem.

Why Choose APIMatic’s Docs as Code?
+++++++++++++++++++++++++++++++++++
APIMatic’s Docs as Code empowers teams to:
 * **Automate Documentation**: Generate docs automatically from API definitions (OpenAPI, RAML, etc.).
 * **Version Control**: Manage documentation alongside your code using Git.
 * **Collaborate Effectively**: Enable developers and technical writers to work together seamlessly.
 * **Customize and Brand**: Tailor your documentation to match your brand’s look and feel.
 * **Integrate with CI/CD**: Automate doc generation and deployment using your favorite tools (GitHub Actions, GitLab CI, Jenkins, etc.).

Get Started in 4 Simple Steps
++++++++++++++++++++++++++++++

Step 1: Install the APIMatic CLI
--------------------------------

 1. Install the APIMatic CLI tool to generate and manage your documentation:

 ::

   npm install -g @apimatic/cli  

 2. Verify the installation  

 ::

   apimatic --version

Step 2: Set Up Your Project Structure
--------------------------------------

APIMatic’s Docs as Code uses a clean, organized directory structure:

 ::

   📂 my-api-docs/  
   ├── spec/               # API definitions (OpenAPI, RAML)  
   ├── content/            # Markdown files + toc.yml  
   ├── static/             # Images, PDFs  
   └── APIMATIC-BUILD.json # Build configuration

 1. Create your project directory:
   
   ::

      mkdir my-api-docs  
      cd my-api-docs  

 2. Add your API definition file (e.g., openapi.json) to the spec/ folder.

Step 3: Configure Your Documentation
-------------------------------------

 1. Create an APIMATIC-BUILD.json file to define your documentation settings:

 ::

  {  
   "theme": "dark",  
   "primaryColor": "#2E7D32",  
   "codeSamples": {  
   "languages": ["python", "java", "csharp"]  
    }  
   }

 * **theme**: Choose a theme (e.g., dark, light).
 * **primaryColor**: Set your brand’s primary color.
 * **codeSamples**: Specify languages for code snippets. 

 Add Markdown content to the content/ folder, e.g., index.md:

 ::

   # Welcome to My API Documentation  

   ## Getting Started  
   1. Install the required tools.  
   2. Write your API definition.  
   3. Generate and publish your docs.  

 2. Define navigation with a toc.yml file:

  ::

   - title: Home  
     path: index.md  
   - title: API Reference  
     path: api-reference.md  

Step 4: Generate and Preview Your Docs
---------------------------------------

 1. Generate your documentation locally:
 
  ::

    apimatic generate --api-definition ./spec/openapi.json 


 This creates a generated-docs/ folder with your static site.

 2. Preview your docs locally:

 ::

   cd generated-docs  
   python -m http.server 8000

  
 Open http://localhost:8000 in your browser to see your documentation in action.


Automate with CI/CD
++++++++++++++++++++

APIMatic integrates seamlessly with your CI/CD pipeline to automate documentation generation and deployment.

Example: GitHub Actions Workflow
--------------------------------

 1. Create a .github/workflows/generate-docs.yml file:

 ::
    
    name: Generate Docs  
   on: [push]  
   jobs:  
   generate:  
    runs-on: ubuntu-latest  
    steps:  
      - uses: actions/checkout@v4  
      - name: Generate Docs  
        run: |  
          apimatic generate --api-definition ./spec/openapi.json  
      - name: Deploy to GitHub Pages  
        uses: peaceiris/actions-gh-pages@v3  
        with:  
          github_token: ${{ secrets.GITHUB_TOKEN }}  
          publish_dir: ./generated-docs

Push your changes to GitHub, and your docs will be automatically generated and deployed to GitHub Pages.


Key Features to Explore
++++++++++++++++++++++++

Async API Generation
--------------------
For large APIs, use async generation to avoid CI/CD timeouts:

 ::

   curl -X POST "https://api.apimatic.io/v1/async-generate" \  
  -H "Authorization: Bearer $APIMATIC_TOKEN" \  
  -o job_id.txt  

   # Poll for status  
   JOB_ID=$(cat job_id.txt)  
   while true; do  
    STATUS=$(curl -s "https://api.apimatic.io/v1/jobs/$JOB_ID" | jq -r .status)  
    if [ "$STATUS" = "completed" ]; then break; fi  
    sleep 10  
   done

Custom Branding
---------------
Add custom CSS/JS to match your brand:

 ::

   {  
  "theme": {  
    "customCSS": "styles/overrides.css"  
   }  
  }  


Versioning
----------
Tag your docs with your API version:

 ::

   API_VERSION=$(jq '.info.version' spec/openapi.json -r)  
   git tag "docs-$API_VERSION"  
   git push --tags 


Why APIMatic?
+++++++++++++

 * **Scalable Workflows**: Clear separation of API definitions, content, and assets.

 * **Developer-Friendly**: Markdown-first authoring and reusable content.

 * **Flexible Integration**: Works with GitHub, GitLab, Jenkins, Azure DevOps, and more. 

 * **Customizable**: Themes, code samples, and branding tailored to your needs.

 * **Reliable**: Versioning, backups, and async generation for large APIs.


Way Forward
++++++++++++

 * Explore **shared partials** for reusable content.
 * Use **breadcrumbs** and **cross-linking** to improve navigation.
 * Migrate legacy docs using APIMatic’s migration tools:

   :: 

      apimatic migrate --source swagger.json --format apimatic


|

Get Started Today
-----------------

Ready to transform your API documentation workflow? Visit `APIMatic’s Docs as Code Documentation <https://docs.apimatic.io/docs-as-code/documentation-as-code-overview/>`_ to learn more or reach out to our team for a `personalized demo <https://www.apimatic.io/request-demo>`_.
