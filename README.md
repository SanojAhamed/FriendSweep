<img width="1672" height="940" alt="FriendSweep" src="https://github.com/user-attachments/assets/968d28de-2a43-4416-bb1b-0c1ce7d926a3" />




# 🧹 FriendSweep

A collection of simple JavaScript automation scripts for Facebook.

Currently available:

- 🧹 **Bulk Unfriend**
- 🔜 **Auto Unfollow**
- 🔜 **Auto Ungroup**
- 🔜 More Facebook automation coming soon...

> ⚡ Fast, simple, and runs directly in your browser. No installation required.

## 🚀 Bulk Unfriend

### How to use

1. Open your **Facebook Friends** page.
2. Press **F12** → open **Console**.
3. Copy the script below.
4. Paste it into the Console and press **Enter**.
5. The script will automatically:
   - Click **More**
   - Click **Unfriend**
   - Click **Confirm**
   - Repeat until the limit is reached.

### ⚙️ Change the limit

At the top of the script:

```js
const MAX = 250;
```

Change it to whatever you want:

```js
const MAX = 10;   // Test
const MAX = 100;  // 100 friends
const MAX = 250;  // 250 friends
const MAX = 500;  // 500 friends
```

### 🧹 Script

```js
(async () => {
  const MAX = 250;

  const DELAY = 100;
  const SCROLL_DELAY = 100;
  const UI_DELAY = 100;

  const sleep = ms =>
    new Promise(resolve => setTimeout(resolve, ms));

  let count = 0;
  let failures = 0;

  console.log(`🚀 100ms FAST MODE — Maximum: ${MAX}`);

  while (count < MAX && failures < 5) {

    const buttons = [
      ...document.querySelectorAll('[aria-label="More"]')
    ];

    if (!buttons.length) {
      window.scrollBy(0, 700);
      await sleep(SCROLL_DELAY);
      continue;
    }

    const button = buttons[0];

    button.scrollIntoView({
      behavior: "instant",
      block: "center"
    });

    await sleep(UI_DELAY);
    button.click();

    await sleep(UI_DELAY);

    const unfriend = [
      ...document.querySelectorAll('[role="menuitem"]')
    ].find(el =>
      /unfriend/i.test(el.innerText || "")
    );

    if (!unfriend) {
      document.body.click();
      failures++;
      await sleep(200);
      continue;
    }

    unfriend.click();

    await sleep(UI_DELAY);

    const confirm = [
      ...document.querySelectorAll('[aria-label="Confirm"]')
    ].find(el =>
      /confirm/i.test(el.innerText || "")
    );

    if (!confirm) {
      console.log("🛑 Confirmation not found. Stopping.");
      break;
    }

    confirm.click();

    count++;
    failures = 0;

    console.log(`👋 ${count}/${MAX}`);

    await sleep(DELAY);
  }

  console.log(`🏁 Finished: ${count} friends removed.`);
})();
```

### ⚠️ Note

- `100ms` is **fast/aggressive mode**.
- If Facebook responds slowly, increase the delays to `300`–`500ms`.
- Test with `MAX = 10` first.
- Facebook can change its interface, which may cause the script to stop working.
- Reload or close the page to stop the script.

---

⭐ **If FriendSweep helps you, consider giving the repository a star!**

Use responsibly and in accordance with Facebook's rules.
