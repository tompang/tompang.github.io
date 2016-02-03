---
layout: post
title:  "Git common command"
date:   2016-02-03 21:00:42 +0800
categories: git update
---
---

// clone remote repository to local
`git clone https://github.com/Alamofire/Alamofire.git`

// 
`git init` // create a git repository at current directory  
`git add ./` // add files to local git repository  

//commit
`git commit -m 'test'` // commit local  

`git pull` // fetch remote and update local  

`git push -u origin master` //commit to remote  


```rust
lua.set("x", 2);
lua.execute::<()>("x = x + 1").unwrap();
let x: i32 = lua.get("x").unwrap();  // x is equal to 3
```
