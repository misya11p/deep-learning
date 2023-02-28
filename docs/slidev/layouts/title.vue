<template>
    <div class="slidev-layout">
        <div id="header">
            <slot name="event" />
            <slot name="date" />
        </div>
        <div id="title">
            <slot name="title" />
            <slot name="subtitle" />
        </div>
        <div id="author">
            <slot name="author" />
        </div>
    </div>
</template>