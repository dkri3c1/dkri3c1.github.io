<script lang="ts">
  import { onMount, onDestroy } from 'svelte';

  const discordId = import.meta.env.PUBLIC_DISCORD_ID;

  const useLanyard = discordId && discordId !== 'your_discord_id';
  const isConfigured = useLanyard;

  let currentTrack = null;
  let isPlaying = false;
  let isLoading = true;
  let errorMsg = null;
  let intervalId;

  // Format relative time (Traditional Chinese)
  function formatRelativeTime(uts) {
    if (!uts) return '';
    const now = Math.floor(Date.now() / 1000);
    const diff = now - parseInt(uts, 10);
    if (diff < 0) return '剛剛';
    if (diff < 60) return '剛剛';
    if (diff < 3600) return `${Math.floor(diff / 60)} 分鐘前`;
    if (diff < 86400) return `${Math.floor(diff / 3600)} 小時前`;
    return `${Math.floor(diff / 86400)} 天前`;
  }

  async function fetchSpotifyStatus() {
    if (!isConfigured) return;
    try {
      const url = `https://api.lanyard.rest/v1/users/${discordId}`;
      const res = await fetch(url);
      if (!res.ok) {
        if (res.status === 404) {
          throw new Error('找不到該 Discord 使用者 (404)。請確認您已加入 Lanyard Discord 伺服器 (discord.gg/lanyard)！');
        }
        throw new Error('無法取得 Lanyard 狀態');
      }

      const data = await res.json();
      if (!data.success) throw new Error('Lanyard API 請求失敗');

      const lanyardData = data.data;
      isPlaying = lanyardData.listening_to_spotify;

      if (isPlaying && lanyardData.spotify) {
        const spotify = lanyardData.spotify;
        currentTrack = {
          name: spotify.song,
          artist: { '#text': spotify.artist },
          album: { '#text': spotify.album },
          image: [
            { '#text': spotify.album_art_url },
            { '#text': spotify.album_art_url },
            { '#text': spotify.album_art_url }
          ],
          url: `https://open.spotify.com/track/${spotify.track_id}`
        };
      } else {
        currentTrack = null;
      }
      errorMsg = null;
    } catch (e) {
      console.error(e);
      errorMsg = e.message;
    } finally {
      isLoading = false;
    }
  }

  onMount(() => {
    if (isConfigured) {
      fetchSpotifyStatus();
      intervalId = setInterval(fetchSpotifyStatus, 20000); // update every 20s
    } else {
      isLoading = false;
    }
  });

  onDestroy(() => {
    if (intervalId) clearInterval(intervalId);
  });
</script>

{#if !isConfigured}
  <!-- Unconfigured State -->
  <div class="flex flex-col items-center justify-center p-6 text-center border border-dashed border-[var(--line-divider)] rounded-xl bg-black/5 dark:bg-white/5 my-2">
    <div class="text-[#1DB954] mb-3">
      <svg class="w-12 h-12 animate-pulse" viewBox="0 0 24 24" fill="currentColor">
        <path d="M12 2C6.477 2 2 6.477 2 12s4.477 10 10 10 10-4.477 10-10S17.523 2 12 2zm4.586 14.424c-.18.295-.565.387-.86.207-2.377-1.454-5.37-1.783-8.893-.978-.335.076-.668-.135-.744-.47-.076-.335.135-.668.47-.744 3.856-.88 7.15-.5 9.822 1.137.295.18.387.563.205.848zm1.226-2.722c-.226.367-.707.487-1.074.26-2.72-1.672-6.87-2.157-10.077-1.182-.413.125-.85-.107-.975-.52-.125-.413.107-.85.52-.975 3.66-1.11 8.224-.563 11.346 1.353.367.226.488.707.26 1.074zm.107-2.846C14.28 8.784 8.253 8.584 4.736 9.65c-.538.163-1.107-.143-1.27-.68-.163-.538.143-1.107.68-1.27 4.057-1.23 10.718-1.003 14.935 1.5c.484.287.64.91.353 1.393-.287.485-.91.64-1.393.354z"/>
      </svg>
    </div>
    <h3 class="text-base font-bold text-90">Spotify 播放狀態</h3>
    <p class="text-xs text-50 mt-1 max-w-md">
      請在專案根目錄的 <code class="px-1.5 py-0.5 rounded bg-[var(--inline-code-bg)] text-[var(--inline-code-color)] font-mono text-[10px]">.env</code> 檔案中設定您的 Discord ID，讓 Lanyard 回傳 Spotify 狀態：
    </p>

    <div class="mt-4 w-full max-w-2xl text-left">
      <div class="p-3 bg-black/5 dark:bg-white/5 rounded-lg border border-[var(--line-divider)] flex flex-col justify-between">
        <div>
          <h4 class="text-xs font-bold text-[#1DB954] mb-1">Discord Lanyard (免金鑰)</h4>
          <p class="text-[11px] text-50">只會顯示「正在播放 / 沒有播放」兩種狀態。請確保 Discord 已連結 Spotify，且 Lanyard 可讀取到你的帳號。</p>
        </div>
        <pre class="mt-2.5 p-2 bg-[var(--codeblock-bg)] text-[var(--btn-content)] rounded text-[10px] font-mono overflow-x-auto">PUBLIC_DISCORD_ID=您的DiscordID</pre>
      </div>
    </div>
  </div>
{:else if isLoading}
  <!-- Loading State -->
  <div class="flex flex-col items-center justify-center py-10 my-2">
    <div class="relative w-12 h-12">
      <div class="absolute inset-0 rounded-full border-4 border-t-[#1DB954] border-r-transparent border-b-transparent border-l-transparent animate-spin"></div>
      <div class="absolute inset-1.5 rounded-full border-4 border-b-[#1DB954] border-t-transparent border-r-transparent border-l-transparent animate-spin-reverse"></div>
    </div>
    <span class="text-xs text-50 mt-4 animate-pulse">載入 Spotify 音樂狀態中...</span>
  </div>
{:else if errorMsg && !currentTrack}
  <!-- Error State -->
  <div class="flex flex-col items-center justify-center p-6 text-center text-red-500 my-2">
    <svg class="w-8 h-8 mb-2" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"/>
    </svg>
    <p class="text-xs">載入音樂狀態發生錯誤：{errorMsg}</p>
    <button class="mt-3 btn-regular px-4 py-1 rounded-lg text-xs" on:click={fetchSpotifyStatus}>重新載入</button>
  </div>
{:else}
  <!-- Main Widget Content -->
  <div class="w-full flex flex-col gap-6 py-2">
    <!-- Header/Now Playing -->
    <div class="flex flex-col sm:flex-row items-center sm:items-start gap-6">
      
      <!-- Vinyl Player Disk representation -->
      <div class="relative w-24 h-24 sm:w-28 sm:h-28 flex-shrink-0">
        <!-- Vinyl Record Disk background -->
        <div class="absolute inset-0 bg-neutral-950 rounded-full shadow-xl flex items-center justify-center border-4 border-neutral-800 transition duration-500
          {isPlaying ? 'animate-spin-slow shadow-[#1db954]/10 dark:shadow-[#1db954]/5' : ''}">
          <!-- Vinyl grooves -->
          <div class="absolute inset-2 rounded-full border border-neutral-800/40"></div>
          <div class="absolute inset-4 rounded-full border border-neutral-800/60"></div>
          <div class="absolute inset-6 rounded-full border border-neutral-800/80"></div>
          
          <!-- Album Art in the middle -->
          <div class="w-11 h-11 sm:w-13 sm:h-13 rounded-full overflow-hidden border border-neutral-950 z-10 relative bg-neutral-900 shadow-inner">
            {#if currentTrack && currentTrack.image && currentTrack.image[2]['#text']}
              <img src={currentTrack.image[2]['#text']} alt="Album Art" class="w-full h-full object-cover" />
            {:else}
              <div class="w-full h-full flex items-center justify-center bg-neutral-800 text-neutral-500">
                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm1 16h-2v-2h2v2zm0-4h-2V7h2v7z"/>
                </svg>
              </div>
            {/if}
          </div>
          
          <!-- Vinyl Center Hole spindle center -->
          <div class="absolute w-2 h-2 rounded-full bg-[var(--card-bg)] z-20 shadow-inner"></div>
        </div>

        <!-- Pulse Ring overlay when playing -->
        {#if isPlaying}
          <div class="absolute -inset-1 rounded-full border border-[#1DB954] opacity-35 animate-ping pointer-events-none"></div>
        {/if}
      </div>

      <!-- Info Area -->
      <div class="flex-grow text-center sm:text-left min-w-0 w-full flex flex-col justify-between h-full pt-1">
        <div>
          <!-- Badge status -->
          <div class="flex items-center justify-center sm:justify-start gap-2 mb-2.5">
            {#if isPlaying}
              <span class="relative flex h-2 w-2">
                <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-[#1DB954] opacity-75"></span>
                <span class="relative inline-flex rounded-full h-2 w-2 bg-[#1DB954]"></span>
              </span>
              <span class="text-[10px] font-bold text-[#1DB954] tracking-wider uppercase flex items-center gap-1.5">
                Spotify 正在播放
              </span>
            {:else}
              <span class="h-2 w-2 rounded-full bg-neutral-400 dark:bg-neutral-600"></span>
              <span class="text-[10px] font-bold text-50 tracking-wider uppercase">
                Spotify 上次播放
              </span>
            {/if}
          </div>

          <!-- Song Title -->
          {#if currentTrack}
            <h4 class="text-lg sm:text-xl font-bold text-90 truncate hover:text-[var(--primary)] transition duration-200">
              <a href={currentTrack.url} target="_blank" rel="noopener noreferrer" class="hover:underline">
                {currentTrack.name}
              </a>
            </h4>
            <p class="text-sm sm:text-base text-75 mt-0.5 truncate">{currentTrack.artist['#text']}</p>
            <p class="text-xs text-50 mt-0.5 truncate italic">{currentTrack.album['#text'] || 'Single'}</p>
          {:else}
            <h4 class="text-lg font-bold text-30 my-1">目前未播放音樂</h4>
            <p class="text-xs text-50">開啟 Spotify 並播放音樂時，這裡會即時更新。</p>
          {/if}
        </div>

        <!-- Music Visualizer / Description -->
        <div class="mt-4 flex items-center justify-center sm:justify-start gap-4">
          {#if isPlaying}
            <div class="flex items-end gap-[2px] h-4 w-7 pb-[2px]">
              <div class="bar bar-1"></div>
              <div class="bar bar-2"></div>
              <div class="bar bar-3"></div>
              <div class="bar bar-4"></div>
            </div>
            <span class="text-[10px] text-50 font-medium">Listening on Spotify</span>
          {:else if currentTrack && currentTrack.date}
            <span class="text-[10px] text-50 font-medium">播放於 {formatRelativeTime(currentTrack.date.uts)}</span>
          {:else}
            <span class="text-[10px] text-30 font-medium">Offline</span>
          {/if}
        </div>
      </div>
    </div>

  </div>
{/if}

<style>
  @keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
  }
  .animate-spin-slow {
    animation: spin 12s linear infinite;
  }
  
  @keyframes spin-reverse {
    from { transform: rotate(360deg); }
    to { transform: rotate(0deg); }
  }
  .animate-spin-reverse {
    animation: spin-reverse 2.5s linear infinite;
  }

  @keyframes bounce {
    0%, 100% { transform: scaleY(0.25); }
    50% { transform: scaleY(1); }
  }
  .bar {
    display: inline-block;
    width: 2.5px;
    height: 100%;
    background-color: #1DB954;
    border-radius: 2px;
    transform-origin: bottom;
    transform: scaleY(0.25);
  }
  
  .bar-1 { animation: bounce 1.1s ease-in-out infinite; }
  .bar-2 { animation: bounce 0.75s ease-in-out infinite; animation-delay: 0.12s; }
  .bar-3 { animation: bounce 1.3s ease-in-out infinite; animation-delay: 0.24s; }
  .bar-4 { animation: bounce 0.95s ease-in-out infinite; animation-delay: 0.36s; }
</style>
