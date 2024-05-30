# Environment Setup
npx create-next-app@latest trello-tutorial
- This project will be using TypeScript, ESLint, Tailwind CSS
- no src/ directory, use App Router
- no import alias

[![image.png](https://i.postimg.cc/LXvqCZMn/image.png)](https://postimg.cc/BLLQtbb0)

npx shadcn-ui@latest init
- Use Typescript
- Choose default style
- Neutral as base color
- Use CSS variables for colors

[![image.png](https://i.postimg.cc/rwLhxKMv/image.png)](https://postimg.cc/SJD7pQXr)

**Note:** For Next.js, Node.js version >= v18.17.0 is required.
## Folders Setup
In the `app` folder, create route folder, example: components route
# Marketing Page
Type: `npx shadcn-ui@latest add button`

It will be create ui folder in the `components` button
## Add Logo
Source: [Fonts](https://github.com/emimontanari/trello-dev/blob/main/public/fonts/font.woff2), ![Logo](public/logo.svg)
## Navbar
```tsx
import Link from "next/link";

import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";

export const Navbar = () => {
  return (
    <div className="fixed top-0 w-full h-14 px-4 border-b shadow-sm bg-white flex items-center">
      <div className="md:max-w-screen-2xl mx-auto flex items-center w-full justify-between">
        <Logo />
        <div className="space-x-4 md:block md:w-auto flex items-center justify-between w-full">
          <Button size="sm" variant="outline" asChild>
            <Link href="/sign-in">
              Login
            </Link>
          </Button>
          <Button size="sm" asChild>
            <Link href="/sign-up">
              Get Taskify for free
            </Link>
          </Button>
        </div>
      </div>
    </div>
  );
};

```
[![image.png](https://i.postimg.cc/XqCbsPxT/image.png)](https://postimg.cc/dZwp14bB)
## Description Page
```tsx
import Link from "next/link";
import localFont from "next/font/local";
import { Poppins } from "next/font/google";
import { Medal } from "lucide-react";

import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";

const headingFont = localFont({
  src: "../../public/fonts/font.woff2"
});

const textFont = Poppins({
  subsets: ["latin"],
  weight: [
    "100",
    "200",
    "300",
    "400",
    "500",
    "600",
    "700",
    "800",
    "900"
  ],
});

const MarketingPage = () => {
  return (
    <div className="flex items-center justify-center flex-col">
      <div className={cn(
        "flex items-center justify-center flex-col",
        headingFont.className,
      )}>
        <div className="mb-4 flex items-center border shadow-sm p-4 bg-amber-100 text-amber-700 rounded-full uppercase">
          <Medal className="h-6 w-6 mr-2" />
          No.1 task management
        </div>
        <h1 className="text-3xl md:text-6xl text-center text-neutral-800 mb-6">
          Taskify helps team move
        </h1>
        <div className="text-3xl md:text-6xl bg-gradient-to-r from-fuchsia-600 to-pink-600 text-white px-4 p-2 rounded-md pb-4 w-fit">
          work forward.
        </div>
      </div>
      <div className={cn(
        "text-sm md:text-xl text-neutral-400 mt-4 max-w-xs md:max-w-2xl text-center mx-auto",
        textFont.className,
      )}>
        Collaborate, manage projects, and reach new productivity peaks. From high rises to the home office, the way your team work is unique - accomplish it all with Taskify.
      </div>
      <Button className="mt-6" size="lg" asChild>
        <Link href="/sign-up">
          Get Taskify for free
        </Link>
      </Button>
    </div>
  );
};

export default MarketingPage;

```
[![image.png](https://i.postimg.cc/J0VrCmy1/image.png)](https://postimg.cc/Fdp50Mgq)
## Footer
```tsx
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";

export const Footer = () => {
  return (
    <div className="fixed bottom-0 w-full p-4 border-t bg-slate-100">
      <div className="md:max-w-screen-2xl mx-auto flex items-center w-full justify-between">
        <Logo />
        <div className="space-x-4 md:block md:w-auto flex items-center justify-between w-full">
          <Button size="sm" variant="ghost">
            Privacy Policy
          </Button>
          <Button size="sm" variant="ghost">
            Terms of Service
          </Button>
        </div>
      </div>
    </div>
  );
};

```
[![image.png](https://i.postimg.cc/jd31DcXf/image.png)](https://postimg.cc/QH5mPpvN)
# Full Marketing Layout Page
```tsx
import { Footer } from "./_components/footer";
import { Navbar } from "./_components/navbar";

const MarketingLayout = ({
  children
}: {
  children: React.ReactNode;
}) => {
  return (
    <div className="h-full bg-slate-100">
      <Navbar />
      <main className="pt-40 pb-20 bg-slate-100">
        {children}
      </main>
      <Footer />
    </div>
  );
};

export default MarketingLayout;

```
# Authentication
## Config site
Let's go to add config folder and config site

**site.ts**
```tsx
export const siteConfig = {
  name: "Taskify",
  description: "Collaborate, manage projects, and reach new productivity peaks",
};
```
Change metadata:
```tsx
import { siteConfig } from "@/config/site";

export const metadata: Metadata = {
  title: {
    default: siteConfig.name,
    template: `%s | ${siteConfig.name}`,
  },
  description: siteConfig.description,
  icons: [
    {
      url: "/logo.svg",
      href: "/logo.svg"
    }
  ]
};
```
## Create Sign in page
Let's create sign in page using Clerk, I create Sign in page with login with Google account. Type: `npm install @clerk/nextjs`. Add `.env` file to `.gitignore`, then add `.env` file and paste to set your environment variables.
```.env
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=
```
### Add ClerkProvider to your app
This global provider provides active session and user data to your app within it

I create (platform) folder **app/(platform)/layout.tsx**
```tsx
import { ClerkProvider } from "@clerk/nextjs";

const PlatformLayout = ({
  children
}: {
  children: React.ReactNode;
}) => {
  return (
    <ClerkProvider>
      {children}
    </ClerkProvider>
  );
};

export default PlatformLayout;

```
### Update middleware.ts
Update your middleware file or create one at the root of your project or src/ directory if you're using a src/ directory structure.

The authMiddleware helper enables authentication and is where you'll configure your protected routes.
```ts
import { authMiddleware } from "@clerk/nextjs";

export default authMiddleware({});

export const config = {
  matcher: ["/((?!.+.[w]+$|_next).*)", "/", "/(api|trpc)(.*)"],
};
```
## Build sign in and sign up page
Create a new file that will be used to render the sign-in page. In the file, import the `<SignIn />` component from `@clerk/nextjs` and render it.

**trello-tutorial/app/(platform)/(clerk)/sign-in/[[...sign-in]]/page.tsx**
```tsx
import { SignIn } from "@clerk/nextjs";
 
export default function Page() {
  return <SignIn />;
}
```
We do similarity for Sign up page
```tsx
import { SignUp } from "@clerk/nextjs";
 
export default function Page() {
  return <SignUp />;
}
```
## Update your environments variables
Next, add environment variables for the `signIn`, `signUp`, `afterSignUp`, and `afterSignIn` paths:
```env
NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/
```
Add routes:
```tsx
export default authMiddleware({
  publicRoutes: ["/"]
});
```
## Add div center login page
```tsx
const ClerkLayout = ({ children }: {
  children: React.ReactNode;
}) => {
  return (
    <div className="h-full flex items-center justify-center">
      {children}
    </div>
  )
}

export default ClerkLayout;

```
[![image.png](https://i.postimg.cc/fyLtTWLm/image.png)](https://postimg.cc/DJ9yxT1w)
## Test protected
I am using strict "use client" to test client mode

[![image.png](https://i.postimg.cc/RV264BzJ/image.png)](https://postimg.cc/GHJhx6tb)

As you can see, you can change profile cover, sign out account.

[![image.png](https://i.postimg.cc/tTLJy3KQ/image.png)](https://postimg.cc/34FYFvrn)
```tsx
"use client";

import { UserButton } from "@clerk/nextjs";

const ProtectedPage = () => {
  return (
    <div>
      <UserButton
        afterSignOutUrl="/"
      />
    </div>
  )
}
export default ProtectedPage;

```
# Organizations
## Enable Organization Clerk
[![image.png](https://i.postimg.cc/tRVsTLw9/image.png)](https://postimg.cc/Lh29Q0mW)

Create `select-org/[[...select-org]]/page.tsx` file in the (clerk) folder:
```tsx
import { OrganizationList } from "@clerk/nextjs";

export default function CreateOrganizationPage() {
  return (
    <OrganizationList
      hidePersonal
      afterSelectOrganizationUrl="/organization/:id"
      afterCreateOrganizationUrl="/organization/:id"
    />
  );
};

```
We try to sign in Google account, create organization, set role is admin and press skip button. As you can see, when you click organization, you can see the the switch personal account is hide.

[![image.png](https://i.postimg.cc/zGfj9z2J/image.png)](https://postimg.cc/2VPhWDfJ)
## Change button
```tsx
const buttonVariants = cva(
  "inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        primary: "bg-sky-700 text-primary-foreground hover:bg-sky-700/90"
      }
    }
  }
)
```
## Create navbar page
Logo display is flex, create button rounded, display none, auto height, padding-y is 1.5 and padding x is 2.

Plus button icon size is sm, border radius is 4px, display is block, md is hidden.

Organization Switcher box display is flex, justify content is center and align items is center.

Avatar user button height and width are 30px. After click sign-out button, it will be redirect layout page.
```tsx
import { Plus } from "lucide-react";

import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";
import { OrganizationSwitcher, UserButton } from "@clerk/nextjs";

export const Navbar = () => {
  return (
    <nav className="fixed z-50 top-0 px-4 w-full h-14 border-b shadow-sm bg-white flex items-center">
      {/* TODO: Mobile Sidebar */}
      <div className="flex items-center gap-x-4">
        <div className="hidden md:flex">
          <Logo />
        </div>
        <Button variant="primary" size="sm" className="rounded-sm hidden md:block h-auto py-1.5 px-2">
          Create
        </Button>
        <Button variant="primary" size="sm" className="rounded-sm block md:hidden">
          <Plus className="h-4 w-4" />
        </Button>
      </div>
      <div className="ml-auto flex items-center gap-x-2">
        <OrganizationSwitcher 
          hidePersonal
          afterCreateOrganizationUrl="/organization/:id"
          afterLeaveOrganizationUrl="/select-org"
          afterSelectOrganizationUrl="/organization/:id"
          appearance={{
            elements: {
              rootBox: {
                display: "flex",
                justifyContent: "center",
                alignItems: "center"
              },
            },
          }}
        />
        <UserButton 
          afterSignOutUrl="/"
          appearance={{
            elements: {
              avatarBox: {
                height: 30,
                width: 30,
              }
            }
          }}
        />
      </div>
    </nav>
  );
};

```
# Auth middleware
This code utilizes Clerk for authentication and redirects users based on their login status and organization selection within the application. It ensures users are logged in for non-public routes and directs them to the organization selection page if needed. The middleware configuration specifies which routes this logic applies to.
```tsx
import { NextResponse } from "next/server";
import { authMiddleware, redirectToSignIn } from "@clerk/nextjs";

export default authMiddleware({
  publicRoutes: ["/"],
  afterAuth(auth, req) {
    if (auth.userId && auth.isPublicRoute) {
      let path = "/select-org";

      if (auth.orgId) {
        path = `/organization/${auth.orgId}`;
      }

      const orgSelection = new URL(path, req.url);
      return NextResponse.redirect(orgSelection);
    }

    if (!auth.userId && !auth.isPublicRoute) {
      return redirectToSignIn({ returnBackUrl: req.url });
    }

    if (auth.userId && !auth.orgId && req.nextUrl.pathname !== "/select-org") {
      const orgSelection = new URL("/select-org", req.url);
      return NextResponse.redirect(orgSelection);
    }
  }
});

export const config = {
  matcher: ["/((?!.+.[w]+$|_next).*)", "/", "/(api|trpc)(.*)"],
};
```

[![image.png](https://i.postimg.cc/rpGYTV2z/image.png)](https://postimg.cc/FfRxgQRQ)
# Sidebar
- Type: `npm i usehooks-ts`
- Add the skeleton component: `npx shadcn-ui@latest add skeleton`
- Add in the accordion: `npx shadcn-ui@latest add accordion`

**organizationId layout**
```tsx
import { OrgControl } from "./_components/org-control";

const OrganizationIdLayout = ({
  children
}: {
  children: React.ReactNode;
}) => {
  return (
    <>
      <OrgControl />
      {children}
    </>
  )
}

export default OrganizationIdLayout;
```
**Organization Control**
```tsx
"use client";

import { useEffect } from "react";
import { useParams } from "next/navigation";
import { useOrganizationList } from "@clerk/nextjs";

export const OrgControl = () => {
  const params = useParams();
  const { setActive } = useOrganizationList();

  useEffect(() => {
    if (!setActive) return;

    setActive({
      organization: params.organizationId as string,
    });
  }, [setActive, params.organizationId]);
  
  return null;
};
```
```ts
import { create } from "zustand";

type MobileSidebarStore = {
  isOpen: boolean;
  onOpen: () => void;
  onClose: () => void;
};

export const useMobileSidebar = create<MobileSidebarStore>((set) => ({
  isOpen: false,
  onOpen: () => set({ isOpen: true }),
  onClose: () => set({ isOpen: false }),
}))
```
Import these packages
```tsx
import Link from "next/link";
import { Plus } from "lucide-react";
import { useLocalStorage } from "usehooks-ts";
import { useOrganization, useOrganizationList } from "@clerk/nextjs";

import { Button } from "@/components/ui/button";
import { Separator } from "@/components/ui/separator";
import { Skeleton } from "@/components/ui/skeleton";
import { Accordion } from "@/components/ui/accordion";

import { NavItem, Organization } from "./nav-item";
```
Sidebar component
```tsx
interface SidebarProps {
  storageKey?: string;
};

export const Sidebar = ({
  storageKey = "t-sidebar-state",
}: SidebarProps) => {
  const [expanded, setExpanded] = useLocalStorage<Record<string, any>>(
    storageKey,
    {}
  );

  const {
    organization: activeOrganization,
    isLoaded: isLoadedOrg
  } = useOrganization();
  const {
    userMemberships,
    isLoaded: isLoadedOrgList
  } = useOrganizationList({
    userMemberships: {
      infinite: true,
    }
  });

  const defaultAccordionValue: string[] = Object.keys(expanded)
    .reduce((acc: string[], key: string) => {
      if (expanded[key]) {
        acc.push(key);
      }

      return acc;
  }, []);

  const onExpand = (id: string) => {
    setExpanded((curr) => ({
      ...curr,
      [id]: !expanded[id],
    }));
  };

  if (!isLoadedOrg || !isLoadedOrgList || userMemberships.isLoading) {
    return (
      <>
        <Skeleton />
      </>
    );
  }

  return (
    <>
      <div className="font-medium text-xs flex items-center mb-1">
        <span className="pl-4">
          Workspaces
        </span>
        <Button
          asChild
          type="button"
          size="icon"
          variant="ghost"
          className="ml-auto"
        >
          <Link href="/select-org">
            <Plus
              className="h-4 w-4"
            />
          </Link>
        </Button>
      </div>
      <Accordion
        type="multiple"
        defaultValue={defaultAccordionValue}
        className="space-y-2"
      >
        {userMemberships.data.map(({ organization }) => (
          <NavItem
            key={organization.id}
            isActive={activeOrganization?.id === organization.id}
            isExpanded={expanded[organization.id]}
            organization={organization as Organization}
            onExpand={onExpand}
          />
        ))}
      </Accordion>
    </>
  );
};
```

Add protocol and hostname for remote patterns

**next.config.mjs**
```mjs
/** @type {import('next').NextConfig} */
const nextConfig = {
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "img.clerk.com",
      }
    ]
  }
};

export default nextConfig;
```
Add nav items: **nav-items.tsx**
```tsx
"use client";

import { useRouter, usePathname } from "next/navigation";
import Image from "next/image";
import {
  Activity,
  CreditCard,
  Layout,
  Settings,
} from "lucide-react";

import { cn } from "@/lib/utils";
import {
  AccordionContent,
  AccordionItem,
  AccordionTrigger
} from "@/components/ui/accordion";
import { Button } from "@/components/ui/button";

export type Organization = {
  id: string;
  slug: string;
  imageUrl: string;
  name: string;
};

interface NavItemProps {
  isExpanded: boolean;
  isActive: boolean;
  organization: Organization;
  onExpand: (id: string) => void;
};

export const NavItem = ({
  isExpanded,
  isActive,
  organization,
  onExpand,
}: NavItemProps) => {
  const router = useRouter();
  const pathname = usePathname();

  const routes = [
    {
      label: "Boards",
      icon: <Layout className="h-4 w-4 mr-2" />,
      href: `/organization/${organization.id}`,
    },
    {
      label: "Activity",
      icon: <Activity className="h-4 w-4 mr-2" />,
      href: `/organization/${organization.id}/activity`,
    },
    {
      label: "Settings",
      icon: <Settings className="h-4 w-4 mr-2" />,
      href: `/organization/${organization.id}/settings`,
    },
    {
      label: "Billing",
      icon: <CreditCard className="h-4 w-4 mr-2" />,
      href: `/organization/${organization.id}/billing`,
    },
  ];

  const onClick = (href: string) => {
    router.push(href);
  };

  return (
    <AccordionItem
      value={organization.id}
      className="border-none"
    >
      <AccordionTrigger
        onClick={() => onExpand(organization.id)}
        className={cn(
          "flex items-center gap-x-2 p-1.5 text-neutral-700 rounded-md hover:bg-neutral-500/10 transition text-start no-underline hover:no-underline",
          isActive && !isExpanded && "bg-sky-500/10 text-sky-700"
        )}
      >
        <div className="flex items-center gap-x-2">
          <div className="w-7 h-7 relative">
            <Image
              fill
              src={organization.imageUrl}
              alt="Organization"
              className="rounded-sm object-cover"
            />
          </div>
          <span className="font-medium text-sm">
            {organization.name}
          </span>
        </div>
      </AccordionTrigger>
      <AccordionContent className="pt-1 text-neutral-700">
        {routes.map((route) => (
          <Button
            key={route.href}
            size="sm"
            onClick={() => onClick(route.href)}
            className={cn(
              "w-full font-normal justify-start pl-10 mb-1",
              pathname === route.href && "bg-sky-500/10 text-sky-700"
            )}
            variant="ghost"
          >
            {route.icon}
            {route.label}
          </Button>
        ))}
      </AccordionContent>
    </AccordionItem>
  );
};
```
## Mobile Sidebar
- Add separator: `npx shadcn-ui@latest add separator`
- Install zustand: `npm i zustand`
- Add sheet: `npx shadcn-ui@latest add sheet`
- Create hooks folder -> `use-mobile-sidebar.ts`
```ts
import { create } from "zustand";

type MobileSidebarStore = {
  isOpen: boolean;
  onOpen: () => void;
  onClose: () => void;
};

export const useMobileSidebar = create<MobileSidebarStore>((set) => ({
  isOpen: false,
  onOpen: () => set({ isOpen: true }),
  onClose: () => set({ isOpen: false }),
}));
```
**mobile-sidebar.tsx**
```tsx
"use client";

import { Menu } from "lucide-react";
import { useEffect, useState } from "react";
import { usePathname } from "next/navigation";

import { useMobileSidebar } from "@/hooks/use-mobile-sidebar";
import { Button } from "@/components/ui/button";
import { Sheet, SheetContent } from "@/components/ui/sheet";

import { Sidebar } from "./sidebar";

export const MobileSidebar = () => {
  const pathname = usePathname();
  const [isMounted, setIsMounted] = useState(false);

  const onOpen = useMobileSidebar((state) => state.onOpen);
  const onClose = useMobileSidebar((state) => state.onClose);
  const isOpen = useMobileSidebar((state) => state.isOpen);

  useEffect(() => {
    setIsMounted(true);
  }, []);

  useEffect(() => {
    onClose();
  }, [pathname, onClose])

  if (!isMounted) {
    return null;
  }

  return (
    <>
      <Button
        onClick={onOpen}
        className="block md:hidden mr-2"
        variant="ghost"
        size="sm"
      >
        <Menu className="h-4 w-4" />
      </Button>
      <Sheet open={isOpen} onOpenChange={onClose}>
        <SheetContent
          side="left"
          className="p-2 pt-10"
        >
          <Sidebar
            storageKey="t-sidebar-mobile-state"
          />
        </SheetContent>
      </Sheet>
    </>
  )
}
```
# Workspace Settings
Add skeleton NavItem:
```tsx
import { Skeleton } from "@/components/ui/skeleton";

NavItem.Skeleton = function SkeletonNavItem() {
  return (
    <div className="flex items-center gap-x-2">
      <div className="w-10 h-10 relative shrink-0">
        <Skeleton className="h-full w-full absolute" />
      </div>
      <Skeleton className="h-10 w-full" />
    </div>
  );
};
```
Add three loading:
```tsx
<div className="flex items-center justify-between mb-2">
  <Skeleton className="h-10 w-[50%]" />
  <Skeleton className="h-10 w-10" />
</div>
<div className="space-y-2">
  <NavItem.Skeleton />
  <NavItem.Skeleton />
  <NavItem.Skeleton />
</div>
```
Add settings page: Go to [organizationId]/settings/page.tsx, remove box shadow, width 100%
```tsx
import { OrganizationProfile } from "@clerk/nextjs";

const SettingsPage = () => {
  return (
    <div className="w-full">
      <OrganizationProfile
        appearance={{
          elements: {
            rootBox: {
              boxShadow: "none",
              width: "100%"
            },
            card: {
              border: "1px solid #e5e5e5",
              boxShadow: "none",
              width: "100%"
            }
          }
        }}
      />
    </div>
  );
};

export default SettingsPage;
```

[![image.png](https://i.postimg.cc/c44wmsH0/image.png)](https://postimg.cc/rz3Dy2xP)
# Server Actions
Install prisma: `npm i -D prisma`

Initial prisma: `npx prisma init`

We will use Neon tech to create postgresSQL, create account then create database: `trello-tutorial`

Create initial schema:
```prisma
// prisma/schema.prisma
datasource db {
  provider  = "postgresql"
  url  	    = env("DATABASE_URL")
  // uncomment next line if you use Prisma <5.10
  // directUrl = env("DATABASE_URL_UNPOOLED")
}

generator client {
  provider = "prisma-client-js"
}

model Board {
  id String @id @default(uuid())

  title String
}
```

- Type generate: `npx prisma generate`
- Push database: `npx prisma db push`

Install prisma client: `npm i @prisma/client`

In the lib folder, create db.ts and declare global
```ts
import { PrismaClient } from "@prisma/client";

declare global {
  var prisma: PrismaClient | undefined;
};

export const db = new PrismaClient();

if (process.env.NODE_ENV !== "production") globalThis.prisma = db;
```
Create input text:
```tsx
import { db } from "@/lib/db";

const OrganizationIdPage = () => {
  async function create(formData: FormData) {
    "use server";

    const title = formData.get("title") as string;

    await db.board.create({
      data: {
        title,
      }
    });
  }

  return (
    <div>
      <form action={create}>
        <input
          id="title"
          name="title"
          required
          placeholder="Enter a board title"
          className="border-black border p-1"
        />
      </form>
    </div>
  );
};
```
Now let's type `npx prisma studio`, it will be redirect to `localhost:5555`, type test and press enter

[![image.png](https://i.postimg.cc/tJCTb4Z5/image.png)](https://postimg.cc/8J9DBDbJ)

Install zod: `npm i zod`, create actions folder -> `create-board.ts`

Import these packages:
```tsx
"use server";

import { z } from "zod";

import { db } from "@/lib/db";
import { revalidatePath } from "next/cache";

const CreateBoard = z.object({
  title: z.string(),
});

export async function create(formData: FormData) {
  const { title } = CreateBoard.parse({
    title: formData.get("title"),
  });

  await db.board.create({
    data: {
      title,
    }
  });

  revalidatePath("/organization/org_2fPDxjUvciNnN8KvLmvqzMKeCrF");
}
```
Set title and button:
```tsx
import { create } from "@/actions/create-board";
import { Button } from "@/components/ui/button";
import { db } from "@/lib/db";
import { Board } from "./board";

const OrganizationIdPage = async () => {
  const boards = await db.board.findMany();

  return (
    <div className="flex flex-col space-y-4">
      <form action={create}>
        <input
          id="title"
          name="title"
          required
          placeholder="Enter a board title"
          className="border-black border p-1"
        />
        <Button type="submit">
          Submit
        </Button>
      </form>
      <div className="space-y-2">
        {boards.map((board) => (
          <Board key={board.id} title={board.title} id={board.id} />
        ))}
      </div>
    </div>
  );
};
```
Let's go to add interface and delete button
```tsx
import { Button } from "@/components/ui/button";

interface BoardProps {
  title: string;
  id: string;
}

export const Board = ({
  title,
  id
}: BoardProps) => {
  return (
    <form className="flex items-center gap-x-2">
      <p>
        Board title: {title}
      </p>
      <Button type="submit" variant="destructive" size="sm">
        Delete
      </Button>
    </form>
  )
}
```
Add delete action: **actions/delete-board.ts**
```ts
"use server";

import { db } from "@/lib/db";
import { revalidatePath } from "next/cache";

export async function deleteBoard(id: string) {
  await db.board.delete({
    where: {
      id
    }
  });

  revalidatePath("/organization/org_2fPDxjUvciNnN8KvLmvqzMKeCrF");
}
```
Add message required letters:`
```ts
import { db } from "@/lib/db";
import { revalidatePath } from "next/cache";
import { redirect } from "next/navigation";

export type State = {
  errors?: {
    title?: string[];
  },
  message?: string | null;
}

const CreateBoard = z.object({
  title: z.string().min(3, {
    message: "Minimum length of 3 letters is required"
  })
});

export async function create(prevState:State, formData: FormData) {
  const validatedFields = CreateBoard.safeParse({
    title: formData.get("title"),
  });

  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
      message: "Missing fields."
    }
  }

  const { title } = validatedFields.data;

  try {
    await db.board.create({
      data: {
        title,
      }
    });
  } catch(error) {
    return {
      message: "Database Error",
    }
  }

  revalidatePath("/organization/org_2fPDxjUvciNnN8KvLmvqzMKeCrF");
  redirect("/organization/org_2fPDxjUvciNnN8KvLmvqzMKeCrF");
}
```
## Form Input
```tsx
"use client";

import { Input } from "@/components/ui/input";
import { useFormStatus } from "react-dom";

interface FormInputProps {
  errors?: {
    title?: string[];
  }
}

export const FormInput = ({ errors }: FormInputProps) => {
  const { pending } = useFormStatus();

  return (
    <div>
      <Input
        id="title"
        name="title"
        required
        placeholder="Enter a board title"
      disabled={pending}
      />
      {errors?.title ? (
        <div>
          {errors.title.map((error: string) => (
            <p key={error} className="text-rose-500">
              {error}
            </p>
          ))}
        </div>
      ) : null}
    </div>
  );
};
```
## Form Button
```tsx
import { Button } from "@/components/ui/button"
import { useFormStatus } from "react-dom"

export const FormButton = () => {
  const { pending } = useFormStatus();

  return (
    <Button disabled={pending} type="submit">
      Submit
    </Button>
  )
}
```
## Form Delete
```tsx
import { Button } from "@/components/ui/button"
import { useFormStatus } from "react-dom"

export const FormButton = () => {
  const { pending } = useFormStatus();

  return (
    <Button disabled={pending} type="submit">
      Submit
    </Button>
  )
}
```
## Form Components
```tsx
"use client";

import { create } from "@/actions/create-board";
import { Button } from "@/components/ui/button";
import { useFormState } from "react-dom";
import { FormInput } from "./form-input";
import { FormButton } from "./form-button";

export const Form = () => {
  const initialState = { message: null, errors: {} };
  const [state, dispatch] = useFormState(create, initialState);

  return (
    <form action={dispatch}>
      <div className="flex flex-col space-y-2">
        <FormInput errors={state?.errors} />
      </div>
      <FormButton />
    </form>
  )
}
```
# useActions abstractions
## Create safe actions library
```ts
import { z } from "zod";

export type FieldErrors<T> = {
  [K in keyof T]?: string[];
};

export type ActionState<TInput, TOutput> = {
  fieldErrors?: FieldErrors<TInput>;
  error?: string | null;
  data?:TOutput;
};

export const createSafeAction = <TInput, TOutput>(
  schema: z.Schema<TInput>,
  handler: (validateData: TInput) => Promise<ActionState<TInput, TOutput>>
) => {
  return async (data: TInput): Promise<ActionState<TInput, TOutput>> => {
    const validationResult = schema.safeParse(data);
    if (!validationResult.success) {
      return {
        fieldErrors: validationResult.error.flatten().fieldErrors as FieldErrors<TInput>,
      };
    }

    return handler(validationResult.data);
  }
}
```
## Set title required action

**actions/create-board/schema.ts**
```ts
import { z } from "zod";

export const CreateBoard = z.object({
  title: z.string({
    required_error: "Title is required",
    invalid_type_error: "Title is required",
  }).min(3, {
    message: "Title is too short."
  }),
});
```
## Add types actions

**actions/create-board/types.ts**
```ts
import { z } from "zod";
import { Board } from "@prisma/client";

import { ActionState } from "@/lib/create-safe-action";

import { CreateBoard } from "./schema";

export type InputType = z.infer<typeof CreateBoard>;
export type ReturnType = ActionState<InputType, Board>;
```
**useAction state**
```ts
import { useState, useCallback } from "react";

import { ActionState, FieldErrors } from "@/lib/create-safe-action";

type Action<TInput, TOutput> = (data: TInput) => Promise<ActionState<TInput, TOutput>>;

interface UseActionOptions<TOutput> {
  onSuccess?: (data: TOutput) => void;
  onError?: (error: string) => void;
  onComplete?: () => void;
};

export const useAction = <TInput, TOutput> (
  action: Action<TInput, TOutput>,
  options: UseActionOptions<TOutput> = {}
) => {
  const [fieldErrors, setFieldErrors] = useState<FieldErrors<TInput> | undefined>(
    undefined
  );
  const [error, setError] = useState<string | undefined>(undefined);
  const [data, setData] = useState<TOutput | undefined>(undefined);
  const [isLoading, setIsLoading] = useState<boolean>(false);

  const execute = useCallback(
    async (input: TInput) => {
      setIsLoading(true);

      try {
        const result = await action(input);

        if (!result) {
          return;
        }

        if (result.fieldErrors) {
          setFieldErrors(result.fieldErrors);
        }

        if (result.error) {
          setError(result.error);
          options.onError?.(result.error);
        }

        if (result.data) {
          setData(result.data);
          options.onSuccess?.(result.data);
        }
      } finally {
        setIsLoading(false);
        options.onComplete?.();
      }
    },
    [action, options]
  );

  return {
    execute,
    fieldErrors,
    error,
    data,
    isLoading
  };
};
```
## Check error and success
```tsx
const { execute, fieldErrors } = useAction(createBoard, {
  onSuccess: (data) => {
    console.log(data, "SUCCESS!");
  },
  onError: (error) => {
    console.error(error);
  },
});
```
throw new error:
```ts
"use server";

import { auth } from "@clerk/nextjs";
import { revalidatePath } from "next/cache";

import { db } from "@/lib/db";
import { createSafeAction } from "@/lib/create-safe-action";

import { InputType, ReturnType } from "./types";
import { CreateBoard } from "./schema";

const handler = async (data: InputType): Promise<ReturnType> => {
  const { userId } = auth();

  if (!userId) {
    return {
      error: "Unauthorized",
    };
  }

  const { title } = data;

  let board;

  try {
    throw new Error("balbala");
    board = await db.board.create({
      data: {
        title,
      }
    });
  } catch (error) {
    return {
      error: "Failed to create."
    }
  }

  revalidatePath(`/board/${board.id}`);
  return { data: board };
};

export const createBoard = createSafeAction(CreateBoard, handler);
```