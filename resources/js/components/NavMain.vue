<script setup lang="ts">
import { Collapsible, CollapsibleContent, CollapsibleTrigger } from '@/components/ui/collapsible';
import {
    SidebarGroup,
    SidebarGroupLabel,
    SidebarMenu,
    SidebarMenuButton,
    SidebarMenuItem,
    SidebarMenuSub,
    SidebarMenuSubItem,
    useSidebar,
} from '@/components/ui/sidebar';
import { type NavItem, type SharedData } from '@/types';
import { Link, usePage } from '@inertiajs/vue3';
import { ChevronDownIcon } from 'lucide-vue-next';

defineProps<{
    items: NavItem[];
}>();

const page = usePage<SharedData>();
const { open, setOpen } = useSidebar();
</script>

<template>
    <SidebarGroup class="px-2 py-0">
        <SidebarGroupLabel>Platform</SidebarGroupLabel>
        <SidebarMenu>
            <div v-for="item in items" :key="item.title">
                <Collapsible v-if="item.children" :defaultOpen="item.children.some((child) => child.href === page.url)" class="group/collapsible">
                    <SidebarMenuItem>
                        <CollapsibleTrigger as-child>
                            <SidebarMenuButton class="cursor-pointer justify-between font-normal" @click.prevent="setOpen(true)" :tooltip="item.title">
                                <div class="flex items-center gap-2">
                                    <component :is="item.icon" class="h-4 w-4" />
                                    <span v-show="open">{{ item.title }}</span>
                                </div>
                                <ChevronDownIcon v-show="open" class="group-data-[state=open]/collapsible:rotate-180" />
                            </SidebarMenuButton>
                        </CollapsibleTrigger>
                        <CollapsibleContent>
                            <SidebarMenuSub>
                                <SidebarMenuSubItem v-for="child in item.children" :key="child.title">
                                    <SidebarMenuButton as-child :is-active="child.href === page.url">
                                        <Link v-if="child.href" :href="child.href">
                                            <component :is="child.icon" />
                                            <span>{{ child.title }}</span>
                                        </Link>
                                        <span v-else>
                                            <component :is="child.icon" />
                                            <span>{{ child.title }}</span>
                                        </span>
                                    </SidebarMenuButton>
                                </SidebarMenuSubItem>
                            </SidebarMenuSub>
                        </CollapsibleContent>
                    </SidebarMenuItem>
                </Collapsible>
                <SidebarMenuItem v-else>
                    <SidebarMenuButton as-child :is-active="item.href === page.url" :tooltip="item.title">
                        <Link v-if="item.href" :href="item.href">
                            <component :is="item.icon" />
                            <span>{{ item.title }}</span>
                        </Link>
                        <span v-else class="flex items-center gap-2">
                            <component :is="item.icon" />
                            <span>{{ item.title }}</span>
                        </span>
                    </SidebarMenuButton>
                </SidebarMenuItem>
            </div>
        </SidebarMenu>
    </SidebarGroup>
</template>
