## Install jekyll
Jekyll is a static website generator that is supported by github

https://jekyllrb.com/docs/installation/

## Modify the website
First, let's run the website locally. CD to your folder and type

    jekyll serve    
 
#### Root pages
If you want to add a new ROOT page (such as contact, fundings, ...), copy and paste one that you like and modify it.
Don't forget to modify the METADATA at the top of the page, for example:

    ---
    layout: noimage
    group: navigation
    linkname: contact
    title: Contact Us - Needlefinder
    pagetitle: Contact Us
    ---
    
- layout: noimage or default (if you want an image on top) - the layouts are defined in the _layouts folder. 
You can create a new one if you like
- group: if navigation, a link will appear in the menu of the website
- linkname: the link name in the menu
- title: HTML page title (displayed by Google in the search results)
- pagetitle: the title at the top of the page
- it's done. Check locally if that works <http://localhost:4001>

#### Members pages
- Copy and paste the page of a lab member, found in the folder: members
- Give it a name such as firstname_lastname.html
- Edit the _data/people.yaml file and add the linkname of your page

    - name: Guillaume Pernelle
      position: PhD student 
      image: guillaume_pernelle.jpg
      link: guillaume_pernelle
      
- it's done. Check locally if that works <http://localhost:4001/members/firstname_lastname>

#### Publications
- Edit the file _data/publications.yaml
- it's done. Check locally if that works <http://localhost:4001/publications>

#### Codes
- Edit the file _data/codes.yaml
- it's done. Check locally if that works <http://localhost:4001/code>