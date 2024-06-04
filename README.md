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
# useAction abstractions
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
# Form Components
Add label: `npx shadcn-ui@latest add label`
## Add error label
**components/form/form-input.tsx**

```tsx
"use client";

import { forwardRef } from "react";
import { useFormStatus } from "react-dom";

import { cn } from "@/lib/utils";
import { Label } from "@/components/ui/label";
import { Input } from "@/components/ui/input";

import { FormErrors } from "./form-errors";

interface FormInputProps {
  id: string;
  label?: string;
  type?: string;
  placeholder?: string;
  required?: boolean;
  disabled?: boolean;
  errors?: Record<string, string[] | undefined>;
  className?: string;
  defaultValue?: string;
  onBlur?: () => void;
}

export const FormInput = forwardRef<HTMLInputElement, FormInputProps>(({
  id,
  label,
  type,
  placeholder,
  required,
  disabled,
  errors,
  className,
  defaultValue = "",
  onBlur
}, ref) => {
  const { pending } = useFormStatus();

  return (
    <div className="space-y-2">
      <div className="space-y-1">
        {label ? (
          <Label
            htmlFor={id}
            className="text-xs font-semibold text-neutral-700"
          >
            {label}
          </Label>
        ) : null}
        <Input
          onBlur={onBlur}
          defaultValue={defaultValue}
          ref={ref}
          required={required}
          name={id}
          id={id}
          placeholder={placeholder}
          type={type}
          disabled={pending|| disabled}
          className={cn(
            "text-sm px-2 py-1 h-7",
            className,
          )}
          aria-describedby={`${id}-error`}
        />
      </div>
      <FormErrors
        id={id}
        errors={errors}
      />
    </div>
  )
});

FormInput.displayName = "FormInput";
```
## Add circle error icon
We will use XCircle:

**components/form/form-errors.tsx**
```tsx
import { XCircle } from "lucide-react";

interface FormErrorsProps {
  id: string;
  errors?: Record<string, string[] | undefined>;
};

export const FormErrors = ({
  id,
  errors
}: FormErrorsProps) => {
  if (!errors) {
    return null;
  }

  return (
    <div
      id={`${id}-error`}
      aria-live="polite"
      className="mt-2 text-xs text-rose-500"
    >
      {errors?.[id]?.map((error: string) => (
        <div 
          key={error}
          className="flex items-center font-medium p-2 border border-rose-500 bg-rose-500/10 rounded-sm"
        >
          <XCircle className="h-4 w-4 mr-2" />
          {error}
        </div>
      ))}
    </div>
  )
}
```
## Form Submit

**components/form/form-submit.tsx**
```tsx
"use client";

import { useFormStatus } from "react-dom";

import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";

interface FormSubmitProps {
  children: React.ReactNode;
  disabled?: boolean;
  className?: string;
  variant?: "default" | "destructive" | "outline" | "secondary" | "ghost" | "link" | "primary";
};

export const FormSubmit = ({
  children,
  disabled,
  className,
  variant
}: FormSubmitProps) => {
  const { pending } = useFormStatus();

  return (
    <Button
      disabled={pending || disabled}
      type="submit"
      variant={variant}
      size="sm"
      className={cn(className)}
    >
      {children}
    </Button>
  )
}
```
Save button:
```tsx
<FormSubmit>
  Save
</FormSubmit>
```

[![image.png](https://i.postimg.cc/pX9nsbB3/image.png)](https://postimg.cc/dLY10fp9)
# Board Popover Form
## Info label
Let's go to add info label with organization image cover, name organization and credit card icon:
```tsx
<div className="flex items-center gap-x-4">
  <div className="w-[60px] h-[60px] relative">
    <Image
      fill
      src={organization?.imageUrl!}
      alt="Organization"
      className="rounded-md object-cover"
    />
  </div>
  <div className="space-y-1">
    <p className="font-semibold text-xl">
      {organization?.name}
    </p>
    <div className="flex items-center text-xs text-muted-foreground">
      <CreditCard className="h-3 w-3 mr-1" />
      Free
    </div>
  </div>
</div>
```
Add info loading:
```tsx
Info.Skeleton = function SkeletonInfo() {
  return (
    <div className="flex items-center gap-x-4">
      <div className="w-[60px] h-[60px] relative">
        <Skeleton className="w-full h-full absolute" />
      </div>
      <div className="space-y-2">
        <Skeleton className="h-10 w-[200px]" />
        <div className="flex items-center">
          <Skeleton className="h-4 w-4 mr-2" />
          <Skeleton className="h-4 w-[100px]" />
        </div>
      </div>
    </div>
  )
}
```
## Board List
- Type: `npx shadcn-ui@latest add tooltip`
- Type: `npx shadcn-ui@latest add popover`
Hint components:
```tsx
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger
} from "@/components/ui/tooltip";

interface HintProps {
  children: React.ReactNode;
  description: string;
  side?: "left" | "right" | "top" | "bottom";
  sideOffset?: number;
};

export const Hint = ({
  children,
  description,
  side = "bottom",
  sideOffset = 0
}: HintProps) => {
  return (
    <TooltipProvider>
      <Tooltip delayDuration={0}>
        <TooltipTrigger>
          {children}
        </TooltipTrigger>
        <TooltipContent
          sideOffset={sideOffset}
          side={side}
          className="text-xs max-w-[220px] break-words"
        >
          {description}
        </TooltipContent>
      </Tooltip>
    </TooltipProvider>
  )
}
```

Let's go to add Board List with `popover` components, get hint and help circle icon
```tsx
import { HelpCircle, User2 } from "lucide-react";

import { Hint } from "@/components/hint";
import { FormPopover } from "@/components/form/form-popover";

export const BoardList = () => {
  return (
    <div className="space-y-4">
      <div className="flex items-center font-semibold text-lg text-neutral-700">
        <User2 className="h-6 w-6 mr-2" />
        Your boards
      </div>
      <div className="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4">
        <FormPopover sideOffset={10} side="right">
          <div
            role="button"
            className="aspect-video relative h-full w-full bg-muted rounded-sm flex flex-col gap-y-1 items-center justify-center hover:opacity-75 transition"
          >
            <p className="text-sm">Create new board</p>
            <span className="text-xs">
              5 remaining
            </span>
            <Hint
              sideOffset={40}
              description={`
                Free Workspaces can have up to 5 open boards. For unlimited boards upgrade this workspace.
              `}
            >
              <HelpCircle
                className="absolute bottom-2 right-2 h-[14px] w-[14px]"
              />
            </Hint>
          </div>
        </FormPopover>
      </div>
    </div>
  )
}
```
## Add popover close
Edit popover.tsx
```tsx
const PopoverClose = PopoverPrimitive.Close

export { Popover, PopoverTrigger, PopoverContent, PopoverClose }
```
Add X button from "lucid-react":
```tsx
<PopoverClose asChild>
  <Button
    className="h-auto w-auto p-2 absolute top-2 right-2 text-neutral-600"
    variant="ghost"
  >
    <X className="h-4 w-4" />
  </Button>
</PopoverClose>
```
Create button with text input:
```tsx
<form action={onSubmit} className="space-y-4">
  <div className="space-y-4">
    <FormInput
      id="title"
      label="Board title"
      type="text"
      errors={fieldErrors}
    />
  </div>
  <FormSubmit className="w-full">
    Create
  </FormSubmit>
</form>
```
## Add Form Input
```tsx
<form className="space-y-4">
  <div className="space-y-4">
    <FormInput
      id="title"
      label="Board title"
      type="text"
    />
  </div>
</form>
<FormSubmit className="w-full">
  Create
</FormSubmit>
```
Type `npm i sonner`

Add toast message
```tsx
const { execute, fieldErrors } = useAction(createBoard, {
  onSuccess: (data) => {
    console.log({ data });
    toast.success("Board created!");
  },
  onError: (error) => {
    console.log({ error });
    toast.error(error);
  }
});
```

[![image.png](https://i.postimg.cc/J084FZws/image.png)](https://postimg.cc/TLk6Ly3x)
# Board Server Action
## Unsplash API
We will use Unsplash API, register account then go to Your apps -> create new application and copy your access key with format:
```env
NEXT_PUBLIC_UNSPLASH_ACCESS_KEY=
```
Let's go to fetch access key:
```tsx
import { createApi } from 'unsplash-js';

export const unsplash = createApi({
  accessKey: process.env.NEXT_PUBLIC_UNSPLASH_ACCESS_KEY!,
  fetch: fetch,
});
```
## Form Picker
```tsx
"use client";

interface FormPickerProps {
  id: string;
  errors?: Record<string, string[] | undefined>;
};

export const FormPicker = ({
  id,
  errors,
}: FormPickerProps) => {
  return (
    <div>
      Form Picker!
    </div>
  );
};
```
Import Form Picker:
```tsx
<FormPicker
  id="image"
  errors={fieldErrors}
/>
```
Add interface:
```tsx
interface FormPickerProps {
  id: string;
  errors?: Record<string, string[] | undefined>;
};
```
We will set images, loading and `selectedImageId` states, set pending:
```tsx
const { pending } = useFormStatus();

const [images, setImages] = useState<Array<Record<string, any>>>([]);
const [isLoading, setIsLoading] = useState(true);
const [selectedImageId, setSelectedImageId] = useState(null);
```
We will use `useEffect` to fetch images, get random images, set count is 9 (3 rows x 3 columns), finally set is loading is false (mean completed)
```tsx
useEffect(() => {
  const fetchImages = async () => {
    try {
      const result = await unsplash.photos.getRandom({
        collectionIds: ["317099"],
        count: 9,
      });

      if (result && result.response) {
        const newImages = (result.response as Array<Record<string, any>>);
        setImages(newImages);
      } else {
        console.error("Failed to get images from Unsplash");
      }
    } catch (error) {
      console.log(error);
      setImages([]);
    } finally {
      setIsLoading(false);
    }
  };

  fetchImages();
}, []);
```
If is loading, add `Loader2` from `lucide-react`
```tsx
if (isLoading) {
  return (
    <div className="p-6 flex items-center justify-center">
      <Loader2 className="h-6 w-6 text-sky-700 animate-spin" />
    </div>
  );
}
```
Add protocol unsplash images:
```mjs
{
  protocol: "https",
  hostname: "images.unsplash.com",
},
```
Import Form Picker from Form popover then return
```
return (
  <div className="relateive">
    <div className="grid grid-cols-3 gap-2 mb-2">
      {images.map((image) => (
        <div
        key={image.id}
          className={cn(
            "cursor-pointer relative aspect-video group hover:opacity-75 transition bg-muted",
            pending && "opacity-50 hover:opacity-50 cursor-auto"
          )}
          onClick={() => {
            if (pending) return;
            setSelectedImageId(image.id);
          }}
        >
          <Image
            src={image.urls.thumb}
            alt="Unsplash image"
            className="object-cover rounded-sm"
            fill
          />
        </div>
      ))}
    </div>
  </div>
);
```
[![image.png](https://i.postimg.cc/R0HMpJyt/image.png)](https://postimg.cc/2qCRVyzj)

As you can see, each times when click to create new board, the request images was reduce, let's go to add constants folder, create images.ts. F12, Open Network tab then copy json in the response tab

[![image.png](https://i.postimg.cc/FRCDf1FP/image.png)](https://postimg.cc/dkd8psyd)

Import images constants from form picker`
```tsx
const [images, setImages] = useState<Array<Record<string, any>>>(defaultImages);

console.log(error);
setImages(defaultImages);
```
## Add error components
Import check icon from `lucide-react`:
```tsx
import { Check, Loader2 } from "lucide-react";
```

Add alt unsplash image, hyperlink for unsplash username
```tsx
<input
  type="radio"
  id={id}
  name={id}
  className="hidden"
  checked={selectedImageId === image.id}
  disabled={pending}
  value={`${image.id}|${image.urls.thumb}|${image.urls.full}|${image.links.html}|${image.user.name}`}
/>
<Image
  src={image.urls.thumb}
  alt="Unsplash image"
  className="object-cover rounded-sm"
  fill
/>
{selectedImageId === image.id && (
  <div className="absolute inset-y-0 h-full w-full bg-black/30 flex items-center justify-center">
    <Check className="h-4 w-4 text-white" />
  </div>
)}
<Link
  href={image.links.html}
  target="_blank"
  className="opacity-0 group-hover:opacity-100 absolute bottom-0 w-full text-[10px] truncate text-white hover:underline p-1 bg-black/10"
>
  {image.user.name}
</Link>
```

[![image.png](https://i.postimg.cc/rwB1d3m2/image.png)](https://postimg.cc/SnG23ZCg)
## Update databases
```prisma
model Board {
  id                String @id @default(uuid())
  orgId             String
  title             String
  imageId           String
  imageThumbUrl     String @db.Text
  imageFullUrl      String @db.Text
  imageUserName     String @db.Text
  imageLinkHTML     String @db.Text

  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt
}
```
- Reset databases: `npx prisma migrate reset`
- Re-push databases: `npx prisma db push`
## Schema actions
Add error alert images:

**fullstack-trello-clone/actions/create-board/schema.ts**
```ts
image: z.string({
  required_error: "Image is required",
  invalid_type_error: "Image is required",
}),
```
Add required image informations:

**actions/create-board/index.ts**
```ts
const { title, image } = data;

const [
  imageId,
  imageThumbUrl,
  imageFullUrl,
  imageLinkHTML,
  imageUserName
] = image.split("|");

if (!imageId || !imageThumbUrl || !imageFullUrl || !imageUserName || !imageLinkHTML) {
  return {
    error: "Missing fields. Failed to create board."
  };
}
```
Change button to blue: `variant = "primary"`
## Popover action
Add this code when board was created, create board was closed and push id data.

**components/form/form-popover.tsx**
```tsx
const router = useRouter();
const closeRef = useRef<ElementRef<"button">>(null);

const { execute, fieldErrors } = useAction(createBoard, {
  onSuccess: (data) => {
    toast.success("Board created!");
    closeRef.current?.click();
    router.push(`/board/${data.id}`);
  },
  onError: (error) => {
    toast.error(error);
  }
});
```
Let's go to wrap popover create button for desktop and mobile:

**navbar.tsx**
```tsx
<FormPopover>
  <Button variant="primary" size="sm" className="rounded-sm hidden md:block h-auto py-1.5 px-2">
    Create
  </Button>
</FormPopover>
<FormPopover>
  <Button variant="primary" size="sm" className="rounded-sm block md:hidden">
    <Plus className="h-4 w-4" />
  </Button>
</FormPopover>
```

[![image.png](https://i.postimg.cc/XYjPXZr0/image.png)](https://postimg.cc/svLmNgPn)
# Board List
Set auth with await find many boards:
```tsx
const { orgId } = auth();

if (!orgId) {
  return redirect("/select-org");
}

const boards = await db.board.findMany({
  where: {
    orgId,
  },
  orderBy: {
    createdAt: "desc"
  }
});
```

Set match board with Link component with key and href `board.id` background image is thumbnail url image. Paragraph is title board.
```tsx
{boards.map((board) => (
  <Link
    key={board.id}
    href={`/board/${board.id}`}
    className="group relative aspect-video bg-no-repeat bg-center bg-cover bg-sky-700 rounded-sm h-full w-full p-2 overflow-hidden"
    style={{ backgroundImage: `url(${board.imageThumbUrl})` }}
  >
    <div className="absolute inset-0 bg-black/30 group-hover:bg-black/40 transition" />
    <p className="relative font-semibold text-white">
      {board.title}
    </p>
  </Link>
))}
```

Add render loading board list:
```tsx
BoardList.Skeleton = function SkeletonBoardList() {
  return (
    <div className="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-4 gap-4">
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
      <Skeleton className="aspect-video h-full w-full p-2" />
    </div>
  );
};
```

**organization/[organizationId]/page.tsx**

Add Suspense from react with fallback `BoardList` Skeleton:
```tsx
<Suspense fallback={<BoardList.Skeleton />}>
  <BoardList />
</Suspense>
```

[![image.png](https://i.postimg.cc/W32dvCL6/image.png)](https://postimg.cc/CZQhCvKz)
# Board Page
- Install: `npm i lodash`, `npm i -D @types/lodash`

Render title generate metadata in the tab browser:
```tsx
import { startCase } from "lodash";
import { auth } from "@clerk/nextjs";

export async function generateMetadata() {
  const { orgSlug } = auth();

  return {
    title: startCase(orgSlug || "organization"),
  }
}
```

[![image.png](https://i.postimg.cc/02qXYCvR/image.png)](https://postimg.cc/F1Dxm31D)

## Create Board Layout
Render unsplash image:
```tsx
<div
  className="relative h-full bg-no-repeat bg-cover bg-center"
  style={{ backgroundImage: `url(${board.imageFullUrl})` }}
>
  <main className="relative pt-28 h-full">
    {children}
  </main>
</div>
```
## Fetch the board
Let's go to render the title board:
```tsx
export async function generateMetadata({
  params
}: {
  params: { boardId: string; };
}) {
  const { orgId } = auth();

  if (!orgId) {
    return {
      title: "Board",
    };
  }

  const board = await db.board.findUnique({
    where: {
      id: params.boardId,
      orgId
    }
  });

  return {
    title: board?.title || "Board"
  }
}
```

[![image.png](https://i.postimg.cc/Wbr21Vmm/image.png)](https://postimg.cc/xcfr6W4q)

## Create navigation
Let's go to create component: `BoardNavbar`

Define props with data:
```tsx
import { Board } from "@prisma/client";

interface BoardNavbarProps {
  data: Board;
}
```

**_components/board-navbar.tsx**
```tsx
export const BoardNavbar = async ({
  data
}: BoardNavbarProps) => {
  return (
    <div className="w-full h-14 z-[40] bg-black/50 fixed top-14 flex items-center px-6 gap-x-4 text-white">
      Board Navbar!
    </div>
  );
};

```
This code will be render title navbar board.

[![image.png](https://i.postimg.cc/Wp59qq8T/image.png)](https://postimg.cc/LhZkrXGb)
## Board title form
Render title board:

**_components/board-title-form.tsx**
```tsx
"use client";

import { Board } from "@prisma/client";

import { Button } from "@/components/ui/button";

interface BoardTitleFormProps {
  data: Board;
};

export const BoardTitleForm = ({
  data,
}: BoardTitleFormProps) => {
  return (
    <Button
      className="font-bold text-lg h-auto w-auto p-1 px-2"
    >
      {data.title}
    </Button>
  );
};

```
Render pass data: `<BoardTitleForm data={data} />`

Add variant button: `transparent: "bg-transparent text-white hover:bg-white/20"`

Add refs for input, form, set state is editing is false. Form input element when click title is allow editing, hover when blurring
```tsx
const formRef = useRef<ElementRef<"form">>(null);
const inputRef = useRef<ElementRef<"input">>(null);

const [isEditing, setIsEditing] = useState(false);

const enableEditing = () => {
  setIsEditing(true);
  setTimeout(() => {
    inputRef.current?.focus();
    inputRef.current?.select();
  })
}

const disableEditing = () => {
  setIsEditing(false);
};

const onSubmit = (formData: FormData) => {
  const title = formData.get("title") as string;
  console.log("I am submitted", title);
}

if (isEditing) {
  return (
    <form action={onSubmit} ref={formRef} className="flex items-center gap-x-2">
      <FormInput
        ref={inputRef}
        id="title"
        onBlur={() => {}}
        defaultValue={data.title}
        className="text-lg font-bold px-[7px] py-1 h-7 bg-transparent focus-visible:outline-none focus-visible:ring-transparent border-none"
      />
    </form>
  )
}
```
Remove outline: `focus-visible:ring-offset-0`
## Wrap board title on blur
```tsx
const onBlur = () => {
  formRef.current?.requestSubmit();
}
```
## Update board
Update board title schema:

**actions/update-board/schema.ts**
```tsx
import { z } from "zod";

export const UpdateBoard = z.object({
  title: z.string({
    required_error: "Title is required",
    invalid_type_error: "Title is required",
  }).min(3, {
    message: "Title is too short",
  }),
  id: z.string(),
});
```
Update board title types:

**actions/update-board/types.ts**
```ts
import { z } from "zod";
import { Board } from "@prisma/client";

import { ActionState } from "@/lib/create-safe-action";

import { UpdateBoard } from "./schema";

export type InputType = z.infer<typeof UpdateBoard>;
export type ReturnType = ActionState<InputType, Board>;
```

**index.ts**
```ts
"use server";

import { auth } from "@clerk/nextjs";
import { revalidatePath } from "next/cache";

import { db } from "@/lib/db";
import { createSafeAction } from "@/lib/create-safe-action";

import { UpdateBoard } from "./schema";
import { InputType, ReturnType } from "./types";

const handler = async (data: InputType): Promise<ReturnType> => {
  const { userId, orgId } = auth();

  if (!userId || !orgId) {
    return {
      error: "Unauthorized",
    };
  }

  const { title, id } = data;
  let board;

  try {
    board = await db.board.update({
      where: {
        id,
        orgId,
      },
      data: {
        title,
      },
    });
  } catch (error) {
    return {
      error: "Failed to update."
    }
  }

  revalidatePath(`/board/${id}`);
  return { data: board };
};

export const updateBoard = createSafeAction(UpdateBoard, handler);
```
## Toast message
```tsx
const [title, setTitle] = useState(data.title);

const { execute } = useAction(updateBoard, {
  onSuccess: (data) => {
    toast.success(`Board "${data.title}" updated!`);
    setTitle(data.title);
    disableEditing();
  },
  onError: (error) => {
    toast.error(error);
  }
});
```
## Board options

Let's copy and change name folder is `delete-board` and change handler when delete to redirect organization id.
```tsx
const handler = async (data: InputType): Promise<ReturnType> => {
  const { userId, orgId } = auth();

  if (!userId || !orgId) {
    return {
      error: "Unauthorized",
    };
  }

  const { id } = data;
  let board;

  try {
    board = await db.board.delete({
      where: {
        id,
        orgId,
      },
    });
  } catch (error) {
    return {
      error: "Failed to delete."
    }
  }

  revalidatePath(`/organization/${orgId}`);
  redirect(`/organization/${orgId}`);
};
```

We will use popover UI components, using popover trigger to and more button three dots, popover content is Board actions, add close button and add Delete this board button. When delete this board, execute id, otherwise toast message error.
```tsx
"use client";

import { toast } from "sonner";
import { MoreHorizontal, X } from "lucide-react";

import { deleteBoard } from "@/actions/delete-board";
import { useAction } from "@/hooks/use-action";
import { Button } from "@/components/ui/button";
import {
  Popover,
  PopoverClose,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";

interface BoardOptionsProps {
  id: string;
};

export const BoardOptions = ({ id }: BoardOptionsProps) => {
  const { execute, isLoading } = useAction(deleteBoard, {
    onError: (error) => {
      toast.error(error);
    }
  });

  const onDelete = () => {
    execute({ id });
  };

  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button className="h-auto w-auto p-2" variant="transparent">
          <MoreHorizontal className="h-4 w-4" />
        </Button>
      </PopoverTrigger>
      <PopoverContent
        className="px-0 pt-3 pb-3" 
        side="bottom" 
        align="start"
      >
        <div className="text-sm font-medium text-center text-neutral-600 pb-4">
          Board actions
        </div>
        <PopoverClose>
          <Button
            className="h-auto w-auto p-2 absolute top-2 right-2 text-neutral-600"
            variant="ghost"
          >
            <X className="h-4 w-4" />
          </Button>
        </PopoverClose>
        <Button
          variant="ghost"
          onClick={onDelete}
          disabled={isLoading}
          className="rounded-none w-full h-auto p-2 px-5 justify-start font-normal text-sm"
        >
          Delete this board
        </Button>
      </PopoverContent>
    </Popover>
  );
};
```

[![image.png](https://i.postimg.cc/FKvLbTWJ/image.png)](https://postimg.cc/NKNFBkwg)
# List Component
Update schemas:
```prisma
model Board {
  lists             List[]
}

model List {
  id                String @id @default(uuid())
  title             String
  order             Int

  boardId           String
  board             Board @relation(fields: [boardId], references: [id], onDelete: Cascade)

  cards             Card[]

  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt

  @@index([boardId])
}

model Card {
  index             String @id @default(uuid())
  title             String
  order             Int
  description       String? @db.Text

  listId            String
  list              List @relation(fields: [listId], references: [id], onDelete: Cascade)

  createdAt         DateTime @default(now())
  updatedAt         DateTime @updatedAt

  @@index([listId])
}
```
Type: `npx prisma migrate reset`, `npx prisma db push`, `npx prisma generate`

Add interface with param `boardId`:
```tsx
interface BoardIdPageProps {
  params: {
    boardId: string;
  };
};
```
async params, add auth `orgId`, if not, redirect `organizationId`. Add `await db` to find many where `boardId` and `board`, include cards order by ascending and descending.
```tsx
const { orgId } = auth();

if (!orgId) {
  redirect("/select-org");
}

const lists = await db.list.findMany({
  where: {
    boardId: params.boardId,
    board: {
      orgId,
    },
  },
  include: {
    cards: {
      orderBy: {
        order: "asc",
      },
    },
  },
  orderBy: {
    order: "asc",
  },
});
```
Add types:

**types.ts**
```ts
import { Card, List } from "@prisma/client";

export type ListWithCards = List & { cards: Card[] };

export type CardWithList = Card & { list: List };
```
## List Container
Add interface, export then return `ListForm` component
```tsx
"use client";

import { ListWithCards } from "@/types";
import { ListForm } from "./list-form";

interface ListContainerProps {
  data: ListWithCards[];
  boardId: string;
};

export const ListContainer = ({
  data,
  boardId,
}: ListContainerProps) => {
  return (
    <ol>
      <ListForm />
      <div className="flex-shrink-0 w-1" />
    </ol>
  );
};
```
## List Wrapper
Add interface with children, return `li` element
```tsx
interface ListWrapperProps {
  children: React.ReactNode;
}

export const ListWrapper = ({
  children
}: ListWrapperProps) => {
  return (
    <li className="shrink-0 h-full w-[272px] select-none">
      {children}
    </li>
  );
};
```
## List Form
Add params using `useParams`, add form and input refs from `useRef` of React. Add state `isEditing`. If enable editing, set is true, otherwise, set is false (to disable). When press Escape, disable editing. If is editing, return `ListWrapper` component with form element include `FormInput` component, input is hidden. Add list with form submit, add button `X` from `lucide-react`. Return `ListWrapper` component with form element include Plus button add a list.
```tsx
"use client";

import { Plus, X } from "lucide-react";
import { useParams } from "next/navigation";
import { useEventListener, useOnClickOutside } from "usehooks-ts";
import { useState, useRef, ElementRef } from "react";

import { Button } from "@/components/ui/button";
import { FormInput } from "@/components/form/form-input";
import { FormSubmit } from "@/components/form/form-submit";

import { ListWrapper } from "./list-wrapper";

export const ListForm = () => {
  const params = useParams();

  const formRef = useRef<ElementRef<"form">>(null);
  const inputRef = useRef<ElementRef<"input">>(null);

  const [isEditing, setIsEditing] = useState(false);

  const enableEditing = () => {
    setIsEditing(true);
    setTimeout(() => {
      inputRef?.current?.focus();
    });
  };

  const disableEditing = () => {
    setIsEditing(false);
  };

  const onKeyDown = (e: KeyboardEvent) => {
    if (e.key === "Escape") {
      disableEditing();
    };
  };

  useEventListener("keydown", onKeyDown);
  useOnClickOutside(formRef, disableEditing);

  if (isEditing) {
    return (
      <ListWrapper>
        <form
          ref={formRef}
          className="w-full p-3 rounded-md bg-white space-y-4 shadow-md"
        >
          <FormInput
            ref={inputRef}
            id="title"
            className="text-sm px-2 py-1 h-7 font-medium border-transparent hover:border-input focus:border-input transition"
            placeholder="Enter list title..."
          />
          <input
            hidden
            value={params.boardId}
            name="boardId"
          />
          <div className="flex items-center gap-x-1">
            <FormSubmit>
              Add list
            </FormSubmit>
            <Button
              onClick={disableEditing}
              size="sm"
              variant="ghost"
            >
              <X className="h-5 w-5" />
            </Button>
          </div>
        </form>
      </ListWrapper>
    );
  };

  return (
    <ListWrapper>
      <form
        className="w-full p-3 rounded-md bg-white space-y-4 shadow-md"
      >
        <button
          onClick={enableEditing}
          className="w-full rounded-md bg-white/80 hover:bg-white/50 transition p-3 flex items-center font-medium text-sm"
        >
          <Plus className="h-4 w-4 mr-2" />
          Add a list
        </button>
      </form>
    </ListWrapper>
  )
}
```

## Create List actions
Copy, paste and rename action is `create-list`, same as update board action but 
**schema.ts**
```ts
export const CreateList = z.object({
  title: z.string({
    required_error: "Title is required",
    invalid_type_error: "Title is required",
  }).min(3, {
    message: "Title is too short",
  }),
  boardId: z.string(),
});
```
**types.ts**
```ts
import { z } from "zod";
import { List } from "@prisma/client";

import { ActionState } from "@/lib/create-safe-action";

import { CreateList } from "./schema";

export type InputType = z.infer<typeof CreateList>;
export type ReturnType = ActionState<InputType, List>;
```
**index.ts**
```ts
"use server";

import { auth } from "@clerk/nextjs";
import { revalidatePath } from "next/cache";

import { db } from "@/lib/db";
import { createSafeAction } from "@/lib/create-safe-action";

import { CreateList } from "./schema";
import { InputType, ReturnType } from "./types";

const handler = async (data: InputType): Promise<ReturnType> => {
  const { userId, orgId } = auth();

  if (!userId || !orgId) {
    return {
      error: "Unauthorized",
    };
  }

  const { title, boardId } = data;
  let list;

  try {
    const board = await db.board.findUnique({
      where: {
        id: boardId,
        orgId,
      },
    });

    if (!board) {
      return {
        error: "Board not found",
      };
    }

    const lastList = await db.list.findFirst({
      where: { boardId: boardId },
      orderBy: { order: "desc" },
      select: { order: true },
    });

    const newOrder = lastList ? lastList.order + 1 : 1;

    list = await db.list.create({
      data: {
        title,
        boardId,
        order: newOrder,
      },
    });
  } catch (error) {
    return {
      error: "Failed to create."
    }
  }

  revalidatePath(`/board/${boardId}`);
  return { data: list };
};

export const createList = createSafeAction(CreateList, handler);
```