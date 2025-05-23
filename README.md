# GitHub Pages hosted personal portfolio project 

## Introduction
This is a project for showing project progression and as a personal portfolio display. The intention is to use this frontend to write and format documentation and progress notes for any projects that you want.
[Google place](https://google.com)

## Modification for GitHub pages
React application using Tanstack Router.  
The build command is modified to account for the single page nature of GitHub Pages hosting 
```bash
vite build && cd scripts && npx ts-node generatePagesForGitbub.ts && cd ..  && tsc"
```
Special generator script "./scripts/generatePagesForGitHub.ts", generated a folder/file path i the build output for each Tanstack router Route, with HTML and JS script to redirect back to the '/' page with query-parameters telling Tanstack Router to redirect to e certain page.
When for example going directly to the url 'www.{GitHubName}.GitHub.io/{somePage}', this 'somePage' is not a file path that GitHub pages can take you to, so to get this direct link to work when this page is hit, the HTML and JS on this generated page redirects to '/' with the query-parameter '?GitHubPagesRedirectPath=/somePage', then a handler in the root Route loader redirects to this page withing the control of Tanstack router  
## Pages redirect script
```html
<!DOCTYPE html>
<html lang="en">
<head>
<title>Redirection {route}</title>
 <script>
        window.onload = function() {
            const searchParams = window.location.search.replace("?","");
            const searchParamsAsUriString = (searchParams === '' ? '' : '&' + searchParams); 
            window.location.href = '/?GitHubPagesRedirectPath={route}' + searchParamsAsUriString;
        };
    </script>
</head>
<body>
<h1>Redirecting to ${route}</h1>
</body>
</html>
```

To run this application:

```bash
npm install
npm run start  
```

# Building For Production

To build this application for production:

```bash
npm run build
```

# Setup for GitHub pages

Add the code to a repo hosted in GitHub.

Go into the Repo > Settings > (under "Code and Automation") Pages.

Select Source as "GitHub Actions", and use the workflow script "jekyll-gh-pages.yml" from folder ".github/workflows"

Add secrets into the github actions pipeline for:
```
VITE_GITHUB_DATA_REPO={yourRepoNameForDataStorage}
VITE_GITHUB_DATA_REPO_OWNER={yourGithubUserNameOrUserNameOfDataRepoOwner}
```

## Styling

[//]: # (ToDo: Will use Tailwind )


## Backend
There is no traditional backend for this project. 
The data generated from the Frontend (i.e text, images, documentation etc about a project, and project data) will be stored in a separate GitHub repository.

The data storing structure in the Data repo is:


```
projects/
 ├── projects-meta.json  
 └── project-{id}/ 
   ├── project-meta.json 
   ├── pages-meta.json
   ├── page-sections/
   │  ├── page-{id}-sections.json
   │  └── page-{id}-sections.json  
   └── content/
      ├── markdown/
      │  ├── text-a.md 
      │  └── text-b.md 
      └── images/
         ├── image-a.jpeg 
         └── image-b.jpeg
```

## Projects meta data
File "projects/meta.json" contains metadata about the projects that are created:
```json
{
  "projects": [
    {
      "id": 1,
      "name": "project-a",
      "active": true,
      "revision": "rev_3",
      "projectType": "software | hardware | mechanical | electrical"
    }
  ]
}
```

```typescript
interface ProjectsMeta {
    projects: ProjectMeta[];
}
```
## Project meta data
File "projects/{project-name}/meta.json" contains metadata about the project:
```json
{
  "id": 1,
  "name": "project-a",
  "active": true,
  "revision": "rev_3",
  "projectType": "software | hardware | mechanical | electrical" 
}
```
```typescript
type ProjectRevision = `rev_${number}`;

interface ProjectMeta {
    id: number;
    name: string;
    active: boolean;
    revision: ProjectRevision;
    projectType:string;
}
```

## Page-meta / General subject
File "projects/{project-name}/pages-meta.json" contains information regarding the pages for the given project:
```json
{
  "projectId": 1,
  "pages": [
    {
      "sequenceId": 0,
      "pageName": "page-a",
      "active": true,
      "type": "not included for now, add if needed later"
    }
  ]
}
```
```typescript
interface PageMeta{
    id: number;
    sequenceId: number;
    pageName: string;
    active: boolean;
}

interface PagesMeta {
    projectId: number;
    pages: PageMeta[];
}

```
#### Page SequenceId
The order in which the page should be displayed, in increasing order


## Page-sections
File "projects/{project-name}/page-sections/page-{pageName}-sections.json" contains information regarding the rendering/displaying of the project on the page:
```json
{
  "id": 1,
  "pageId": 1,
  "sections": [
    {
      "sequenceId": 0,
      "sectionTitle": "project-a",
      "active": true,
      "type": "md | image | file | link | code | etc",
      "contentLocation": "path"
    }
  ]
}
```
```typescript
interface PageSection {
    id: number;
    sequenceId: number;
    sectionTitle: string;
    active: boolean;
    type: string;
    contentLocation:string;
}

interface PageSectionsMeta {
    id: number;
    pageId: number;
    sections: PageSection[];
}
```
#### SequenceId
The order in which the section should be displayed, in increasing order
#### ContentLocation
The location in the project folder where the content is located: path is relative to "project-name"

[//]: # (Todo: Maybe add later)
[//]: # (#### ContentLocations)

[//]: # (The locations in the project folder where the content is located if there is multiple resources that is included)
[//]: # (#### Content)

[//]: # (Simple content to be displayed, if it is needed  )
