<template>
    <div class="window">
        <div class="window-content">
            <div class="pane-group">
                <div class="pane-sm sidebar">
                    <nav>
                        <h5 class="nav-group-title">工具栏</h5>
                        <span class="nav-group-item">
                            <span class="icon icon-folder"  @click.native="open()"></span>
                            打开
                      </span>

                    </nav>
                </div>
                <div class="pane text-center">内容</div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'landing-page',
    methods: {
        open() {
            console.log("click");
            alert("hello world")
        }
    }
}
</script>

<style>

    button {
        padding-top: 200px;
    }
</style>
