How to update website/ add a new post...
------------------------------------------

1. make your changes to the files, or create a new post with 
hugo new content content/post/my-NEW-post.md
(note how the folder is 'post' and not 'posts')

2. save the changes (obv) and run the 'hugo' command in the terminal in the main folder of the blog...it will build it and show any errors that could show up...if u want to test it locally u can just do 'hugo server -D' and it will serve it locally for quick tests!

3. run the following 3 lines:
git add .
git commit -m "Add your commit message"
git push origin main

4. go to the repo and see if the updates are there. the vercel should be an orange light indicating it is updating it live. once the green check appears, it is live.

5. check it out online and see if you like it!!!



