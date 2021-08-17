# MyTwitchHub

<p align="center">
<img src="https://img.shields.io/badge/Backend-%20Java | JavaServlet%20-F6922B.svg">
<img src="https://img.shields.io/badge/Frontend-%20 React | AntDesign%20-43dcf2.svg">
<img src="https://img.shields.io/badge/Framework-JavaServlet | node.js %20-ec63a8.svg">
<img src="https://img.shields.io/badge/Database-%20 SQL %20-3de540.svg">
<img src="https://img.shields.io/badge/Deployment-%20AWS EC2%20-DDC7FC.svg">
<img src="https://img.shields.io/badge/Platform-%20Fullstack Web%20-F6F063.svg">
</p>

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 🎬 About the project
<p align="justify"> 
  For those of you not familiar with Pacman, it's a game where Pacman (the yellow circle with a mouth in the above figure) moves around in a maze and tries to eat as many food pellets (the small white dots) as possible, while avoiding the ghosts (the other two agents with eyes in the above figure). If Pacman eats all the food in a maze, it wins.
</p>

![-----------------------------------------------------](https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/rainbow.png)

## 🤖 Tech Stack

* Java
* Java Servlet
* Twitch API
* SQL
* React
* Ant Design 3
* Amazon Web Services

## 📐 Design Doc

<p align="center">
<img src="https://user-images.githubusercontent.com/78308927/129795256-9d09747a-8185-4a7f-b0c6-1a34bb102e1c.jpg" width=800>
</p>

## :fire: Key Features

<p align="justify"> 
  For those of you not familiar with Pacman, it's a game where Pacman (the yellow circle with a mouth in the above figure) moves around in a maze and tries to eat as many food pellets (the small white dots) as possible, while avoiding the ghosts (the other two agents with eyes in the above figure). If Pacman eats all the food in a maze, it wins.
</p>

- **RESTful API using Java servlets**.
- **Retrieve real time data through Twitch API and store in MySQL**
- **Support three search functionality: by top games, by game name, and through favorited collections**.
- **Registered user can save and collect favorite clips/streams/videos**.
- **Content-based reommendation system**.
- **Minimal, content-focused, and clutter-free frontEnd design**.


#### Retrieve real-time data from Twitch using Twitch API

 -  [Twitch API](https://dev.twitch.tv/docs/api/) is a RESTFUL API that lets developers build creative integrations for the broader Twitch community 
 -  For all users, myTwitchHub offers top game display and will allow client to search content by game name, which will fetch data by calling two Twitch APIs: [GetTopGames](https://dev.twitch.tv/docs/api/) and [getGames](https://dev.twitch.tv/docs/api/reference#get-games)
 
```
public class TwitchClient {
    private static final String TOKEN = "Bearer lbuiju146o3hwtrw9dhfqlexwxyw34";
    private static final String CLIENT_ID = "qmw2e95aclsaf4i0pxd71n0kpm1x5e";
    private static final String TOP_GAME_URL = "https://api.twitch.tv/helix/games/top?first=%s";
    private static final String GAME_SEARCH_URL_TEMPLATE = "https://api.twitch.tv/helix/games?name=%s";
    private static final int DEFAULT_GAME_LIMIT = 20;
    private static final String STREAM_SEARCH_URL_TEMPLATE = "https://api.twitch.tv/helix/streams?game_id=%s&first=%s";
    private static final String VIDEO_SEARCH_URL_TEMPLATE = "https://api.twitch.tv/helix/videos?game_id=%s&first=%s";
    private static final String CLIP_SEARCH_URL_TEMPLATE = "https://api.twitch.tv/helix/clips?game_id=%s&first=%s";
    private static final String TWITCH_BASE_URL = "https://www.twitch.tv/";
    private static final int DEFAULT_SEARCH_LIMIT = 20;

    // Build the request URL which will be used when calling Twitch APIs, e.g. https://api.twitch.tv/helix/games/top when trying to get top games.
    private String buildGameURL(String url, String gameName, int limit) {
    ...
    }
    
     private List<Item> getItemList(String data) throws TwitchException {
        ObjectMapper mapper = new ObjectMapper();
        try {
            return Arrays.asList(mapper.readValue(data, Item[].class));
        } catch (JsonProcessingException e) {
            e.printStackTrace();
            throw new TwitchException("Failed to parse item data from Twitch API");
        }
    }

    // Returns the top x streams based on game ID.
    private List<Item> searchStreams(String gameId, int limit) throws TwitchException {
        List<Item> streams = getItemList(searchTwitch(buildSearchURL(STREAM_SEARCH_URL_TEMPLATE, gameId, limit)));
        for (Item item : streams) {
            item.setType(ItemType.STREAM);
            item.setUrl(TWITCH_BASE_URL + item.getBroadcasterName());
        }
        return streams;
    }

    // Returns the top x clips based on game ID.
    private List<Item> searchClips(String gameId, int limit) throws TwitchException {
        List<Item> clips = getItemList(searchTwitch(buildSearchURL(CLIP_SEARCH_URL_TEMPLATE, gameId, limit)));
        for (Item item : clips) {
            item.setType(ItemType.CLIP);
        }
        return clips;
    }

    // Returns the top x videos based on game ID.
    private List<Item> searchVideos(String gameId, int limit) throws TwitchException {
        List<Item> videos = getItemList(searchTwitch(buildSearchURL(VIDEO_SEARCH_URL_TEMPLATE, gameId, limit)));
        for (Item item : videos) {
            item.setType(ItemType.VIDEO);
        }
        return videos;
    }

    public List<Item> searchByType(String gameId, ItemType type, int limit) throws TwitchException {
        List<Item> items = Collections.emptyList();

        switch (type) {
            case STREAM:
                items = searchStreams(gameId, limit);
                break;
            case VIDEO:
                items = searchVideos(gameId, limit);
                break;
            case CLIP:
                items = searchClips(gameId, limit);
                break;
        }

        // Update gameId for all items. GameId is used by recommendation function
        for (Item item : items) {
            item.setGameId(gameId);
        }
        return items;
    }

    public Map<String, List<Item>> searchItems(String gameId) throws TwitchException {
        Map<String, List<Item>> itemMap = new HashMap<>();
        for (ItemType type : ItemType.values()) {
            itemMap.put(type.toString(), searchByType(gameId, type, DEFAULT_SEARCH_LIMIT));
        }
        return itemMap;
    }
```

#### user registeration and authentification
```
 ...
```

#### Searching Algorithms: 
  ##### 1. for all users, search by top games / search by game namae
  ```
  ...
  ```
  
  ##### 2. for registered users, search through favorite collections
```
..
```

#### Save, star and collect favorite items 

```
..
```

#### Content-based recommendation System
  
```
```




