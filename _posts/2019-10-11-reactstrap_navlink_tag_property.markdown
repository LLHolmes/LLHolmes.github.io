---
layout: post
title:      "reactstrap NavLink tag property"
date:       2019-10-11 18:53:59 +0000
permalink:  reactstrap_navlink_tag_property
---


I’m exploring bootstrap for the first time (well, [reactstrap](https://reactstrap.github.io/) to be more specific since it’s a react project) and I started with a simple [Nav component](https://reactstrap.github.io/components/navs/) for my navigation bar.  But of course I wanted my navigation bar to actually navigate and not just *look* like a nav bar.  Therefore I needed to employ React Router as well.  But how to impliment that functionality into reactstrap’s Nav components?  This is where the tag property comes into play.

Basically, the tag property will incorporate the *functionality* of a React Router item into a reactstrap component.  To create my basic Nav, I needed to rename one of the `NavLink`s on import so I didn’t have any "Duplicate identifier” errors.  Then it was as simple as sending the React Router NavLink (RRNavLink) information to the reactstrap NavLink component as a prop:

navbar.tsx:
```
import * as React from 'react';
import { NavLink as RRNavLink } from 'react-router-dom';
import Nav from 'reactstrap/lib/Nav';
import NavItem from 'reactstrap/lib/NavItem';
import NavLink from 'reactstrap/lib/NavLink';

export const NavBar: React.FunctionComponent<{}> = () => {

    return (
      <div className="NavBar">
        <Nav>
          <NavItem >
            <NavLink tag={ RRNavLink } to="/">Home</NavLink>
          </NavItem>
          <NavItem>
            <NavLink tag={ RRNavLink } to=“/about”>About</NavLink>
          </NavItem>
          <NavItem>
            <NavLink tag={ RRNavLink } to="/contact">Contact</NavLink>
          </NavItem>
        </Nav>
      </div>
    );
  
  }
```

As for a strange bit of advice for starting your reactstrap journey?  I actually found the [react-bootstrap](https://react-bootstrap.github.io/components/navs/) docs to be really helpful in giving a better explanation of a bunch of the reactstrap components and properties.  Be careful not to use their code (for example: `Nav.Link` vs. `NavLink`) if you have `reactstrap` installed in your `package.json` file, but the explanations are definitely more complete.

ps.  If you can’t tell, I’m also experimenting with Typescript (seen here: `export const NavBar: React.FunctionComponent<{}>`).  I can’t speak to this yet.  Hopefully I’ll have more on this later…you know…once I actually understand it a tiny bit...
