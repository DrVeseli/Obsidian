## Scroll into view
```
hx-on="htmx:afterSwap: document.getElementById('ID').scrollIntoView({ behavior: 'smooth' })"
```
## Form values 
```
hx-vals='{"chatID": {{.ChatID}}}'
```
## Clear input
```
hx-on="htmx:afterRequest:document.getElementById('messageInput').value = ''"
```
