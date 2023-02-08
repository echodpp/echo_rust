# Command Line Application With Rust 
![Screen Shot 2023-02-01 at 2 18 56 PM](https://user-images.githubusercontent.com/90811429/216141512-d3053fba-9771-4843-911b-9519f4bc2c43.png)

## CRUD is an acronym that stands for Create, Read, Update, and Delete. These are the four basic functions of persistent storage.

In the context of a note-taking application written in Rust, the CRUD functions could be implemented as follows:

- Create: A new note can be created by allowing the user to enter text into a form and then saving it to a database or file.

- Read: The notes can be retrieved from the database or file and displayed to the user, either as a list or individually.

- Update: A user can modify an existing note by editing its text and then saving the changes back to the database or file.

- Delete: A user can delete a note by marking it for deletion and then removing it from the database or file.

It is important to note that the exact implementation of these functions will depend on the specific requirements of the application and the technologies being used.



## install rust

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
## new project called'notes'

```
cargo new notes
```
## run project

```
cargo run
```

## pacakge manager

```
Cargo.toml
```

## Read
    
```
      let mut running = true;
      while running == true {
      let mut buffer = String::new();
      io::stdin().read_line(&mut buffer)?;

      let trimmed_body = buffer.trim();
      if trimmed_body == "" {
        running = false;
      } else if trimmed_body == "/list" {
        let mut stmt = conn.prepare("SELECT id, body from notes")?;
        let mut rows = stmt.query(rusqlite::params![])?;
        while let Some(row) = rows.next()? {
          let id: i32 = row.get(0)?;
          let body: String = row.get(1)?;
          println!("{} {}", id, body.to_string());
        }
      } else {
       conn.execute("INSERT INTO notes (body) values (?1)", [trimmed_body])?;
      }
    }    
   
```     

## delete

``` 
    let trimmed_body = buffer.trim();
    let cmd_split = trimmed_body.split_once(" ");

    let mut cmd = trimmed_body;
    let mut msg = "";
    if cmd_split != None {
        cmd = cmd_split.unwrap().0;
        msg = cmd_split.unwrap().1;
    }

    if cmd == "/del" {
        let id = msg;
        conn.execute("delete from notes where id = (?1)", [id])?;
    }

```

## update

```
    else if cmd == "/edit" {
        let msg_split = msg.split_once(" ").unwrap();
        let id = msg_split.0;
        let body = msg_split.1;

        conn.execute("update notes set body = (?1) where id = (?2)", [body, id])?;
    }
```

## install

```
    cargo install --path .
```
