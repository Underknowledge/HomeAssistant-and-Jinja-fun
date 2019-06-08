# messages

<p>
Be shure to check on the Home Assistant Template editor. 
</p>

## Basics </br>  
 </br>  


```yaml
      - service: notify.xyz
        data_template:
          message: >-
                 {{ [
                 " message 0",
                 " message 1",
                 " message 2",
                 " message 3",
                 " message 4",
                 " message 5",
                 " message 6",
                 " message 7",
                 " message 8",
                 " message 9",
                 " message 10"
                 ] | random }}
```

## output:  </br>  


 returns `message 10`, then  `message 4`, then `message 3`, then `message 10`, then...  </br>  

could be used with `{{now().strftime('%d')}} ` for things like open contests. </br>  


