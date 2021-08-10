# Learn-Git-Branching-Solutions-and-Notes
My Solutions and Notes for the [Learn Git Branching Tutorial](https://learngitbranching.js.org/) by Peter Cottle. 

Project Repository: [Learn Git Branching](https://github.com/pcottle/learnGitBranching)



#### Introduction Sequence - A nicely paced introduction to the majority of git commands
    
1. Introduction to Git Commits
    
     Git commits are lightweight snapshots of a project you are working on, and switching between them is fast. 

     Each time we do a ```git commit``` we create a new snapshot that is tied to a parent commit. 

     Make two commits to complete this level.

     Solution:
     
     ```console
      git commit
      git commit
      ```
            
    
 2. Branching in Git

     Branches are pointers to commits. "Branch early and branch often." A branch essentially says "I want to include the work of this commit and all parent commits.

     Make a new branch called bugFix and switch to that branch.

     Solution 1:

     ```console
     git branch bugFix
     git checkout bugFix
     ```

     Solution 2:

     ```console
     git checkout -b bugFix
      ```

3. Merging in Git

    ```git merge``` creates a special commit that has two unique parents. This allows us to combine the work from two branches into one commit. 

    Make a new branch called bugFix
         
    Checkout the bugFix branch with git checkout bugFix
         
    Commit once
         
    Go back to main with git checkout
         
    Commit another time
         
    Merge the branch bugFix into main with git merge
         
    Solution:

    ```console
    git branch bugFix
    git checkout bugFix
    git commit
    git checkout main
    git commit
    git merge bugFix
    ```

 4. Rebase Introduction

    Rebasing is a second way of combining work. It takes a set of commits, copies, and then plops them down somewhere else. The commit log / history of a repo will be alot cleaner if only rebasing is allowed. 

    Checkout a new branch named bugFix

    Commit once

    Go back to main and commit again

    Check out bugFix again and rebase onto main

    Solution:

    ```console
    git branch bugFix
    git checkout bugFix
    git commit
    git checkout main
    git commit
    git checkout bugFix
    git rebase main
    ```

    
#### Ramping Up - The next serving of 100% git awesomes-ness. Hope you're hungry
    
   1. Detach yo' HEAD

         HEAD is the symbolic name for the currently checked out commit. HEAD (almost) always points to the most recent commit which is reflected in the working tree. Normally HEAD points to a branch name, like bugFix. Detaching HEAD just means attaching it to a commit instead of a branch. 


         This 7 minute video has some good supplemental info on HEAD. https://www.youtube.com/watch?v=ZaI1co-rt9I
         
         ```git show HEAD``` will show you where the HEAD is currently pointed. Detached HEAD just means HEAD does not currently point to the most recent commit. 
         
         To complete this level, let's detach HEAD from bugFix and attach it to the commit instead. Specify this commit by its hash. The hash for each commit is displayed on the circle that represents the commit.
         
         Solution:
         
         ```console
         git checkout C4
         ```

   2. Relative Refs (^)

         ```git log``` is what we will use in the real world to see what our commit tree looks like. Relative refs make it easier to move up or down a relative number of times in the commit tree. ```^``` moves up one level in the commit tree and ```~<num>``` moves upwards the number of times specified.
         
         
         To complete this level, check out the parent commit of bugFix. This will detach HEAD.

         You can specify the hash if you want, but try using relative refs instead!
         
         Solution 1: (relative ref)
         
         ```console
         git checkout bugFix^
         ```
         
         Solution 2: (relative ref)
         
         ```console
         git checkout bugFix~1
         ```
         
         Solution 3: (relative ref)
         
         ```console
         git checkout C4^
         ```
         
         Solution 4: (direct hash)
         
         ```console
         git checkout C3
         ```
         

   3. Relative Refs #2 (^)

         A common use of relative refs is to move branches around You can directly reassign a branch to a commit with the -f option: ```git branch -f main HEAD~3``` will force the main branch to 3 parents behind the HEAD.
         
         To complete this level, move HEAD, main, and bugFix to their goal destinations shown
         
         Solution:
         
         ```console
         git checkout HEAD^
         git branch -f main C6
         git branch -f bugFix HEAD^
         ```
     
   4. Reversing Changes in Git
    
         There are two primary ways to undo changes in git. ```git reset```and ```git revert```
         
         Found a wording issue in this section and ended up submitting a PR for it. https://github.com/pcottle/learnGitBranching/pull/857
         
         ```git reset``` reverses a change by moving a branch backwards in time as if the commit never happened. Open question - what happens to the commit that got erased? Will it still show up if you run git log? Answer: It does not show up in git log anymore, but it seems to still exist, you can still check it out and it comes back to life.  This method works find if you are just working locally, but not so good if you are collaborating with others using a remote repo. 

         ```git revert``` solves that problem. This allows us to reverse changes and share it with others. 
         
         To complete this level, reverse the most recent commit on both local and pushed. You will revert two commits total (one per branch).

         Keep in mind that pushed is a remote branch and local is a local branch -- that should help you choose your methods.
         
         Solution:
         
         ```console
         git reset HEAD^
         git checkout pushed
         git revert pushed
         ```

         
#### Moving Work Around - "Git" comfortable with modifying the source tree :P

    
   1. Cherry-pick Intro
         ```git cherry-pick <commit1> <commit2> <...>``` says you want to grab a series of commits and copy it below your current location (HEAD)

         To complete this level, simply copy some work from the three branches shown into main. You can see which commits we want by looking at the goal visualization.

         Solution:
           
         ```console
         git cherry-pick C3 C4 C7
         ```


   2. Interactive Rebase Intro
      
         git rebase -i will let you reorder the sequence of commits

         Done
    
 
#### A Mixed Bag - A mixed bag of Git techniques, tricks, and tips
    
   1. Grabbing Just 1 Commit
    
       Let's say you are debugging a problem, you go back and add some print debug statements and print statements. Each one of those has their own commit now. Now you solve the problem and you just want your bugFix commit back into the main branch without bringing along your debug commits. That's the use case for this problem. Two potential solutions are suggested: ```git rebase -i``` or ```git cherry-pick```. 

       Solution using cherry-pick

       ```console
       git checkout main
       git cherry-pick bugFix
       ```

       Solution using git rebase -i 
         
       ```console
       git rebase -i c1 // delete commits c2 and c3
       git branch -f c4 main'
       ```
 
    
   2. Juggling Commits
    
       Another use case that happens commonly is we need to change a commit way back in our commit history. 
          
       The section utilizes ```git commit --amend```. A good explanation of that command is at the [Pro Git Book](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History). This is useful for making a small change to your most recent commit, like modifying the commit message. 
          
       Here is the problem: 

       We will overcome this difficulty by doing the following:

       We will re-order the commits so the one we want to change is on top with git rebase -i
         
       We will git commit --amend to make the slight modification
         
       Then we will re-order the commits back to how they were previously with git rebase -i
         
       Finally, we will move main to this updated part of the tree to finish the level (via the method of your choosing)
         
       There are many ways to accomplish this overall goal (I see you eye-ing cherry-pick), and we will see more of them later, but for now let's focus on this technique. Lastly, pay attention to the goal state here -- since we move the commits twice, they both get an apostrophe appended. One more apostrophe is added for the commit we amend, which gives us the final form of the tree

   
       ```console
       git rebase -i C1 
       git checkout newImage 
       git commit --amend 
       git checkout caption
       git rebase -i C1 
       git branch -f main C3 
       git checkout main
       ```
          
   3. Juggling Commits #2

         The reordering done in the prior section can cause rebase conflicts, so we can use git cherry-pick to avoid those. 
    
         git cherry-pick will plop down a commit from anywhere in the tree onto HEAD
            
         The problem: So in this level, let's accomplish the same objective of amending C2 once but avoid using rebase -i. I'll leave it up to you to figure it out! :D
            
         ```console
         git checkout main
         git cherry-pick c2
         git cherry-pick c3
         ```
            
         My solution used 3 commands, their solution used 4 commands. That's a little odd that it worked. It didn't require me to use the git ammend command which I thought I might have to use. 

             
 
    
   4.  Git Tags

          Branches are useful but they are mutable and often changing. Tags are a way to (almost) permanently mark certain commits as milestones.
         
          Solution:
         
          ``` console
          git tag v0 c1
          git tag v1 c2
          git checkout v1
          ```

   5.  Git Describe
    
          ```git describe``` will tell you where you are relative to the closest anchor.
            
          Solution:

          ```console
          git describe
          git describe HEAD
          git describe main
          git describe c5
          git describe c3
          git describe side
          git commit
          ```
    
#### Advanced Topics - For the Truly Brave!
   1. Rebasing over 9000 times

         Rebasing 
         Branches
         Man, we have a lot of branches going on here! Let's rebase all the work from these branches onto main.

         Upper management is making this a bit trickier though -- they want the commits to all be in sequential order. So this means that our final tree should have C7' at the bottom, C6' above that, and so on, all in order.

         Solution:

         ```console
         git rebase c6
         git checkout c3
         git rebase c2'
         git checkout c7
         git rebase c3'
         git checkout c7'
         git rebase -i c0
         git branch -f main c7''
         ```
      
      
   3. Multiple parents

         The ```^``` operator specifies which parent reference to follow from a merge commit.
         
         To complete this level, create a new branch at the specified destination.

         Obviously it would be easy to specify the commit directly (with something like C6), but I challenge you to use the modifiers we talked about instead!
         
         Solution:
         
         ```console
         git branch bugWork main~1^2~1
         ```
         
         Alternate solution but not using the operators taught in this lesson:
         
         ```console
         git branch bugWork C2
         ````
         
         
   4. Branch spaghetti

         WOAHHHhhh Nelly! We have quite the goal to reach in this level.

         Here we have main that is a few commits ahead of branches one two and three. For whatever reason, we need to update these three other branches with modified versions of the last few commits on main.

         Branch one needs a re-ordering of those commits and an exclusion/drop of C5. Branch two just needs a pure reordering of the commits, and three only needs one commit transferred!

         We will let you figure out how to solve this one -- make sure to check out our solution afterwards with show solution.
         
         Solution:
         
         Did it in 12 steps, but need to revise it down.

         First did three git commit --amends to create three branches.
         
         Then use git cherry-pick to move commits under the right branches. Then git rebase-i to reorder the branches. Then git branch -f to get the branch labels to the right commits. 

   #### Push & Pull -- Git Remotes!
      
   1. Clone Intro 

         Git Remotes are a great backup tool and they allow us to easily collaborate with others on projects. It's important to understand the underlying structure of how remotes work. 

         Solution:

         ```console
         git clone
         ``` 

   2. Remote Branches

         Git automatically sets the name of your remote to be origin. In this tutorial we will use o as a shorthand for origin.
         
         To finish this level, commit once off of main and once after checking out o/main. This will help drive home how remote branches behave differently, and they only update to reflect the state of the remote.

         Solution:
         
         ```console
         git commit
         git checkout o/main
         git commit
         ```

   3. Git Fetchin'

         In this lesson we will learn how to fetch data from a remote repository -- the command for this is conveniently named git fetch.

         git fetch essentially brings our local representation of the remote repository into synchronization with what the actual remote repository looks like (right now).

         git fetch doesn't change anything about your local state, it just downloads the updates. 

         Solution:

         ```console
         git fetch
         ```

        
   4. Git Pullin'

         The workflow of fetching commits from a remote and then merging them is so common there is a single command that does both: ```git pull```

         Solution:
         
         ```console
         git pull
         ```

   5. Faking Teamwork

         Go ahead and make a remote (with git clone), fake some changes on that remote, commit yourself, and then pull down those changes. It's like a few lessons in one!

         Solution:

         ```console
         git clone
         git fakeTeamwork 2
         git commit
         git pull
         ```
   6. Git Pushin'

         git push is the opposite of git pull. It allows you to get your code onto the remote to share with the rest of the team. The default behavior of git push with no arguments depends on the settings called push.default.

         Solution:
         ```console
         git commit
         git push
         git push
         ```


   7. Diverged History

         The big challenge with pulling and pushing comes when we have a diverged history, meaning someone else contributed some code to a remote that conflicts with code you are working on. In this case, git won't allow you to git push your code if it conflicts with something that someone else wrote. To resolve this, you must base your work off the most recent version of the remote branch. One solution is to do a ```git rebase``` before pushing. You can also do the same thing with a ```git merge``` command instead of ```git rebase```. Or an even faster way is via ```git pull --rebase```. 

         In order to solve this level, take the following steps:

         Clone your repo

         Fake some teamwork (1 commit)

         Commit some work yourself (1 commit)

         Publish your work via rebasing

         Solution 1:

         ```console
         git clone
         git fakeTeamwork 1
         git commit
         git pull --rebase
         git push
         ```
         
         Solution 2:

         ```console
         git clone
         git fakeTeamwork 1
         git commit
         git fetch
         git rebase o/main
         git push
         ```


   8. Locked Main

         If you work on a large team, you generally can't push directly onto the main branch. The solution is to create your own branch locally, make your changes, then push that change to the remote using a pull request. 

         Solution:

         ```console
         git branch feature
         git checkout feature
         git branch -f main c1
         git push 
         ```
      
      
#### To Origin and Beyond -- Advanced Git Remotes!
      
   1. Push Main!

        This level is pretty hefty -- here is the general outline to solve:

        There are three feature branches -- side1 side2 and side3

        We want to push each one of these features, in order, to the remote

        The remote has since been updated, so we will need to incorporate that work as well
    
       <details><summary><b>Solution</b></summary>
       <p> 

       ```console
       git checkout main
       git pull --rebase
       git cherry-pick c2 c3 c4 c5 c6 c7
       git push
       ```
       
      </p>
      </details>


   2. Merging with Remotes

        Solve the same thing as the last level, but this time use merging instead of rebasing. 
      
      <details><summary><b>Solution</b></summary>
      <p> 

      ```console
      git checkout main
      git pull
      git merge side1
      git merge side2
      git merge side3
      git push
      ```

      </p>
      </details>

   3. Remote Tracking
       Your local main branch doesn't always have to be linked to the remote main branch, although this is the common default setup.
       
       If you want to do something different you create a new branch and point it to whatever remote you want:

       ```git checkout -b totallyNotMain o/main``` Creates a new branch named totallyNotMain and sets it to track o/main.
       
       Another method is to just update the remote tracking on an existing branch with this command:
       
       ```git branch -u o/main foo```
       
       and if foo is the currently check out branch, we don't even need to specify it:

        ```git branch -u o/main```
        
        Ok! For this level let's push work onto the main branch on remote while not checked out on main locally. I'll let you figure out the rest since this is the         advanced course :P
        
         <details><summary><b>Solution</b></summary>
      <p> 

      ```console
      git checkout -b side o/main
      git commit
      git pull --rebase
      git push
      ```

      </p>
      </details>

   4. Git Push Arguments

      git push can take arguments in the form of: ```git push <remote> <place>```
      
      An example: ```git push origin main```, we are telling git what branch to look at to determine where the commits will come from, and where they are going to. Since we specified main as the <place> parameter, git totally ignores where we are checked out when we run this command. 
    
      Ok, for this level let's update both foo and main on the remote. The twist is that git checkout is disabled for this level!

      Note: The remote branches are labeled with o/ prefixes because the full origin/ label does not fit in our UI. Don't worry about this... simply use origin as the name of the remote like normal.
    
     
      <details><summary><b>Solution</b></summary>
      <p> 

      ```console
      git push origin foo
      git push origin main
      ```

      </p>
      </details>
      
   5. Git Push Arguments - Expanded
    
      If you wanted to push some commits from one branch to another branch and they don't have the same name, you can use ```git push origin <source>:<destination>```. This is known as colon refspec, and refspec is just another word for a location that git can figure out. You can even specify the name of a new branch on the remote and git will create it for you. 
    
       We can use relative references like this:  ```git push origin foo^:main```. Or if the destination we want to push to doesn't yet exist, we can write: ```git push origin main:newBranch```
    
      For this level, try to get to the end goal state shown in the visualization, and remember the format of: ```<source>:<destination>```

    
      <details><summary><b>Solution</b></summary>
      <p> 

      ```console
      git push origin foo:main
      git push origin main^:foo
      ```

      </p>
      </details>
    
      
   6. Fetch Arguments
   
      Fetch arguments can also use the same constructs that are available for the push arguments. We can use colon refspec, the <place> parameter and more.

      It you call ```git fetch``` with no arguments, it will download all new commits from the remote onto all the remote branches. 
    
      Ok, enough talking! To finish this level, fetch just the specified commits in the goal visualization. Get fancy with those commands!

      You will have to specify the source and destination for both fetch commands. Pay attention to the goal visualization since the IDs may be switched around!
    
      <details><summary><b>Solution</b></summary>
      <p> 

      ```console
      git fetch origin foo:main
      git fetch origin main~2:foo
      git fetch origin main~1:foo
      git checkout foo
      git merge main
      ```
          
      This solution is 5 steps, they say to try to do it in 4. Maybe combine the last two steps?

      </p>
      </details>
    
   7. Source of Nothing
    
      <details><summary><b>Solution</b></summary>
      <p> 

      ```console
     
      ```

      </p>
      </details>
    
   8. Pull Arguments
    
       <details><summary><b>Solution</b></summary>
       <p> 

       ```console
     
       ```

       </p>
       </details>

         
    
