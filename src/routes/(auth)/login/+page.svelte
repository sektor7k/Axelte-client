<script lang="ts">
    import { Button } from "$lib/components/ui/button";
    import { Input } from "$lib/components/ui/input";
    import { Label } from "$lib/components/ui/label";
    import { Card, CardContent, CardHeader, CardTitle, CardFooter } from "$lib/components/ui/card";
    import axios from "axios";
    import { toast } from "svelte-sonner";
    import { goto } from "$app/navigation";
    let email = "";
    let password = "";

    async function handleSubmit() {
        try {
            const response = await axios.post(`${import.meta.env.VITE_SERVER_URL}/auth/login`, {
                email,
                password
            },
            {
                withCredentials: true, // for write cookie to browser helellel :)
            });
            toast.success("Login successful!");
            goto("/");
        } catch (error:any) {
            toast.error(error.response.data.message|| "Something went wrong!");
        }
    }
</script>

<div class="flex justify-center items-center min-h-screen ">
    <Card class="w-[400px]">
        <CardHeader>
            <CardTitle class="text-2xl font-bold text-center">Welcome Back</CardTitle>
        </CardHeader>
        <CardContent>
            <form on:submit|preventDefault={handleSubmit}>
                <div class="space-y-4">
                    <div class="space-y-2">
                        <Label for="email">Email</Label>
                        <Input 
                            type="email" 
                            id="email" 
                            placeholder="Enter your email" 
                            bind:value={email}
                            required
                        />
                    </div>
                    <div class="space-y-2">
                        <Label for="password">Password</Label>
                        <Input 
                            type="password" 
                            id="password" 
                            placeholder="Enter your password" 
                            bind:value={password}
                            required
                        />
                    </div>
                    <Button type="submit" class="w-full">
                        Sign In
                    </Button>
                </div>
            </form>
        </CardContent>
        <CardFooter class="flex justify-center">
            <p class="text-sm text-gray-500">
                Don't have an account? 
                <a href="/signup" class="text-primary hover:underline">Sign up</a>
            </p>
        </CardFooter>
    </Card>
</div>