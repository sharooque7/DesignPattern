
### Single Responsibility Principle

```
    - Every class, module, or function in a program should have one responsibility/purpose in a program.
    - Seperation of concerns.
    - The below code seperate concerns related to persist data to DB in seperate class.
    - So the Team and PersistData class will each have single responsibility/purpose in the program to follow.
```
 ---
 ```
 public class Team
    {
        public string TeamName { get; set; }
        public string TeamOwner { get; set; }
        public Stats TeamStats { get; set; }

        public Team(string TeamName, string TeamOwner, Stats TeamStats)
        {
            this.TeamName = TeamName;
            this.TeamOwner = TeamOwner;
            this.TeamStats = TeamStats;
        }

    }

    public class PersistData
    {
        public void WriteToDatabase(Team team)
        {
            ///Logic to insert the data into database
        }

    }
```