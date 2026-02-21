# 🧠 Zettelkasten

${table.concat(query[[
  from p = index.tag "page"
  where string.find(p.name, "^zettelkasten/") and p.name ~= "zettelkasten/index"
  order by p.lastModified desc
  limit 20
  select "* [[" .. p.name .. "]]"
]], "\n")}
