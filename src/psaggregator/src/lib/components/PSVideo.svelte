<script lang="ts">
    import { dateFormat } from "$lib/utils/dateFormat";
    import type { ContentPiece } from "@prisma/client";
    import { SHOW_ABSOLUTE_DATES } from "../../config/config";

    export let video: ContentPiece;
    export let lineClamp: 2 | 3 | 4 | 5 | 6 = 2;

    let classes = "";
    export { classes as class };

    $: humanReadableMinutes = video.duration ? Math.floor(video.duration / 60) : null;
    $: humanReadableSeconds = video.duration ? video.duration % 60 : null;
</script>

<style lang="postcss">
    .overlap {
        position: absolute;
        left: 0;
        bottom: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(0deg, rgba(0, 0, 0, 0.6) 0, transparent 40%);
        transition: background 0.2s;
    }

    .line-clamp {
        display: -webkit-box;
        -webkit-box-orient: vertical;
        overflow: hidden;
    }
</style>

<a class="card relative aspect-square h-full w-64 {classes}" href={video.href} target="_blank" title={video.title}>
    <div class="block">
        <div class="relative aspect-video">
            <img class="h-full w-full object-cover" src={video.imageUri} alt={video.title} />
            <div class="overlap"></div>
            {#if humanReadableMinutes !== null && humanReadableSeconds !== null}
                <span class="absolute bottom-0 right-0 m-2 text-xs font-bold text-white"
                    >{("00" + humanReadableMinutes).slice(-2)}:{("00" + humanReadableSeconds).slice(-2)}</span>
            {/if}
        </div>
    </div>
    <div>
        <div class="line-clamp m-4 font-bold" style="-webkit-line-clamp: {lineClamp}">
            {video.title}
        </div>
    </div>
    {#if video.startDate}
        <div class="absolute bottom-0 right-0 m-2 text-sm font-bold">
            <span>{dateFormat(video.startDate, $SHOW_ABSOLUTE_DATES)}</span>
        </div>
    {/if}
</a>
