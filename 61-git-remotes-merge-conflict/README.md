# Esercizio di risoluzione di un merge conflict

**Il tempo massimo in laboratorio per questo esercizio è di _20 minuti_.
Se superato, sospendere l'esercizio e riprenderlo per ultimo!**

Si visiti https://github.com/APICe-at-DISI/OOP-git-merge-conflict-test.
Questo repository contiene due branch: `master` e `feature`

Per ognuna delle seguenti istruzioni, si annoti l'output ottenuto.
Prima di eseguire ogni operazione sul worktree o sul repository,
si verifichi lo stato del repository con `git status`.

1. Si cloni localmente il repository

git clone git@github.com:APICe-at-DISI/OOP-git-merge-conflict-test.git

Cloning into 'OOP-git-merge-conflict-test'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 12 (delta 1), reused 1 (delta 1), pack-reused 8
Receiving objects: 100% (12/12), done.
Resolving deltas: 100% (2/2), done.

cd OOP....
git status //vedo il branch master e che sono in pari con origin.master

2. Ci si assicuri di avere localmente entrambi i branch remoti

git branch (ne vedo solo 1)

git log --oneline --all --graph (vedo tutti i branch)
* bed943f (origin/feature) Print author information
| * 8e0f29c (HEAD -> master, origin/master, origin/HEAD) Change HelloWorld to print the number of available processors
|/
* d956df6 Create .gitignore
* 700ee0b Create HelloWorld

git checkout feature origin/feature (ora ho la testa su origin.feature, ci ho creato un branch locale feature e ci ho spostato la testa)
Switched to a new branch 'feature'
branch 'feature' set up to track 'origin/feature'.

git branch -v
* feature bed943f Print author information
  master  8e0f29c Change HelloWorld to print the number of available processors

git log --oneline --all --graph (vedo tutti i branch)
* bed943f (HEAD -> feature, origin/feature) Print author information
| * 8e0f29c (origin/master, origin/HEAD, master) Change HelloWorld to print the number of available processors
|/
* d956df6 Create .gitignore
* 700ee0b Create HelloWorld

git status (feature non ha nulla di nuovo)

git checkout master //torno al master

git status (master non ha nulla di nuovo)

3. Si faccia il merge di `feature` dentro `master`, ossia: si posizioni la `HEAD` su `master`
   e da qui si esegua il merge di `feature`

git merge feature
Auto-merging HelloWorld.java
CONFLICT (content): Merge conflict in HelloWorld.java
Automatic merge failed; fix conflicts and then commit the result.

4. Si noti che viene generato un **merge conflict**!

-------------------------------------------------------------
public final class HelloWorld {

	private static final String AUTHOR = "Danilo Pianini";

	public static void main(final String[] args) {
<<<<<<< HEAD
		System.out.println("This program is running in a PC with " + procNumber() + " logic processors!");
	}

	public static int procNumber() {
		return Runtime.getRuntime().availableProcessors();
=======
		System.out.println("This program has been realised by " + AUTHOR);
>>>>>>> feature
	}

}
---------------------------------------------------

5. Si risolva il merge conflict come segue:
   - Il programma Java risultante deve stampare sia il numero di processori disponibili
     (funzionalità presente su `master`)
     che il nome dell'autore del file
     (funzionalità presente su `feature`)

-------------------------------------------------------
public final class HelloWorld {

	private static final String AUTHOR = "Danilo Pianini";

	public static void main(final String[] args) {
		System.out.println("This program is running in a PC with " + procNumber() + " logic processors!");
		System.out.println("This program has been realised by " + AUTHOR);
	}

	public static int procNumber() {
		return Runtime.getRuntime().availableProcessors();
	}

}
----------------------------------------------------

git status
On branch master
Your branch is up to date with 'origin/master'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   HelloWorld.java

no changes added to commit (use "git add" and/or "git commit -a")

javac -d bin

java -c ./bin HelloWorld

git status (uguale a prima dato che il .class viene ignorato)

git add HelloWorld.java

git commit --no-edit (uso solo in questo caso dato che il messaggio di base di un merge mi va bene)

git log --oneline --all --graph (figo da vedere)
*   fea70ec (HEAD -> master) Merge branch 'feature'
|\  
| * bed943f (origin/feature, feature) Print author information
* | 8e0f29c (origin/master, origin/HEAD) Change HelloWorld to print the number of available processors
|/  
* d956df6 Create .gitignore
* 700ee0b Create HelloWorld

6. Si crei un nuovo repository nel proprio github personale

creo un nuovo repository su github, dopo averlo creato mi mette davanti al link e le istruzioni per usarlo. Dato che il mio progetto lo ho già non faccio git init ma solo git add.....

7. Si aggiunga il nuovo repository creato come **remote** e si elenchino i remote

git remote -v (vedo tutti i remote //per ora solo origin)
origin  git@github.com:APICe-at-DISI/OOP-git-merge-conflict-test.git (fetch)
origin  git@github.com:APICe-at-DISI/OOP-git-merge-conflict-test.git (push)

git remote add my-test git@github.com:2Skali3/61-git-merge-conflict-test.git (no output)

git remote -v (ora vedo anche il mio)
my-test git@github.com:2Skali3/61-git-merge-conflict-test.git (fetch)
my-test git@github.com:2Skali3/61-git-merge-conflict-test.git (push)
origin  git@github.com:APICe-at-DISI/OOP-git-merge-conflict-test.git (fetch)
origin  git@github.com:APICe-at-DISI/OOP-git-merge-conflict-test.git (push)

8. Si faccia push del branch `master` sul proprio repository

git push my-test master
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 16 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (15/15), 1.56 KiB | 1.56 MiB/s, done.
Total 15 (delta 4), reused 10 (delta 2), pack-reused 0
remote: Resolving deltas: 100% (4/4), done.
To github.com:2Skali3/61-git-merge-conflict-test.git
 * [new branch]      master -> master

---------------
soluzione 2 CREO DIRETTAMENTE UPSTREAM e risolvo anche il punto 9
git push -u my-test master

9. Si setti il branch remoto `master` del nuovo repository come *upstream* per il proprio branch `master` locale

git branch --set-upstream-to=my-test/master
branch 'master' set up to track 'my-test/master'.
