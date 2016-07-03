# SidebarManager
A simple scoreboard manager plugin.

## Configuration
The config file allows you to change the refresh interval of the scoreboard.
```yml
# SidebarManager Configuration

# How many ticks to wait between refreshing the sidebars
# 20 = refresh once every second
# Minimum value is 2
refresh-ticks: 10

```

## API Examples
All API interactions are done through the **ISidebarApi** interface.
```java
ISidebarApi api = SidebarPlugin.getApi();
```
You can register your own sidebars easily.
```java
api.registerSidebar(MySidebar.class);
```
Additionally, you can define a default sidebar to be shown to players when they join the server.
```java
api.setDefaultSidebar(CoolSidebar.class);
api.forceDefaultSidebar(); // Sets the active sidebar of all online players to the default one

api.setDefaultSidebar(null); // Clears the default sidebar
```

You can change a player's active sidebar using the following methods.
```java
api.selectSidebar(player, MyCoolSidebar.class); // Shows the player a sidebar
api.removeSidebar(player); // Hides the sidebar from the player

player.sendMessage("You are using the sidebar: " + api.getActiveSidebar(player).getSimpleName());
```

Making your own sidebar is quite easy. Just extend **Sidebar**.
```java
import me.lucko.sidebarmanager.core.Scoreboard;
import me.lucko.sidebarmanager.core.Sidebar;
import org.bukkit.Material;
import org.bukkit.entity.Player;

public class SimpleSidebar extends Sidebar {

    public SimpleSidebar() {
        super("SimpleSidebar", "&c&lSimple&aSidebar", Material.DIRT, "my.special.sidebarpermission");
    }

    @Override
    protected void onPreUpdateForPlayer(Player player, Scoreboard scoreboard) {
        scoreboard.add("Your Health: ", 3);
        scoreboard.add("> " + player.getHealth(), 2);
        scoreboard.add(" ", 1);
        scoreboard.add("&cEnjoy your stay " + player.getName() + "!", 0);
    }
}
```

## Commands
##### /sidebar
Permission: sidebarmanager.change
* Opens a GUI window, where the user can select which sidebar they want to use.
* Allows users to also hide the sidebar completely.

##### /sidebar reload
Permission: sidebarmanager.reload
* Reschedules the update tasks using the up-to-date value in the config.