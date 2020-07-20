# 2020-CBJHAC-Data-Contest-Submission
I evaluated AHL Defensemen Offensive Zone Aggressiveness Trends using Sportlogiq Data. I focused on even-strength play, as I hypothesized there may be positive or negative diminishing returns with increased defenseman aggressiveness. The data proved challenging because it only included half a season of data, and also the defensemen who are able to produce at a high offensive level are pretty scarce and likely in the NHL. My R Markdown might be easier to read via my [Dropbox](https://www.dropbox.com/home/Portfolio?preview=CBJHAC+Data+Contest+Submission+2019.html), but here are some cool functions, visualizations, and insights:

# Defenseman Clusters 

I found there were 4 distinct offensive zone aggressivness styles of defensemen at even-strength.  

![image](https://user-images.githubusercontent.com/23176357/87902508-aa13d200-ca0e-11ea-915b-0ef7b813acfb.png)

# Plot where a specific player's events occur

```r 
plot_player_events_by_type <- function(player_full_name, strength = "5v5") {
  player_to_plot <- player_full_name
  
  tmp %>%
    filter(player == player_to_plot,
           playzone == "oz",
           skatersonicesituation == strength,
           name %in% c("pass", "reception", "shot", "lpr", "puckprotection", "block")) %>%
    ggplot(aes(x = xadjcoord, y = yadjcoord, color = outcome)) +
    geom_point() +
    gg_rink() + 
    geom_vline(xintercept = 54) +
    coord_flip() +
    facet_wrap(~name) +
    labs(title = paste0(player_full_name, "'s Offensive Zone Locations at ", strength))
}

plot_player_events_by_type("Mitchell Vande Sompel")
```

![image](https://user-images.githubusercontent.com/23176357/87901960-37eebd80-ca0d-11ea-99cd-dbfa1675b0d8.png)


# Relationship Between Pinching and Team Level Expected Goals

At the team level, I looked to see if offensive zone usage of defensemen and percentage of plays below faceoff dots by defensemen have any relationship with possession metrics. The most notable result is a moderately-strong negative relationship between Pinch Contribution Percentage and xGF%. While this is interesting, correlation does not imply causation, so this raises the question if teams that pinch more frequently are using bad strategy, are having their defensemen compensate for a lack of forward talent, or if teams are strategically putting defensemen out of their comfort zone to grow at the AHL level. This is an area for future research.

![image](https://user-images.githubusercontent.com/23176357/87901805-d3cbf980-ca0c-11ea-8322-9b0d86e69eed.png)

# Defensemen are more active when trailing 

Supporting shot frequency research about 3rd period aggressiveness by the trailing team, defensemen take more of an offensive role when trailing late, while teams go into a defensive "shell" when leading late. 

![image](https://user-images.githubusercontent.com/23176357/87902825-6ff70000-ca0f-11ea-8768-1c1696414247.png)

